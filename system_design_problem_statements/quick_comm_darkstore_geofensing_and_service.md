# Dark Store Geofencing & Serviceability Determination

### A system design framework for catchment-area definition, point-in-polygon serviceability, and road-network-aware store assignment in quick commerce

---

## Table of Contents

1. [Reframing the Problem](#1-reframing-the-problem)
2. [Data You Need](#2-data-you-need)
3. [Questions to Ask If This Data Doesn't Exist Yet](#3-questions-to-ask-if-this-data-doesnt-exist-yet)
4. [System Architecture: The Geofencing Pipeline](#4-system-architecture-the-geofencing-pipeline)
5. [Why Straight-Line Radius Geofencing Fails (Direct Answer to the Railway-Line Case)](#5-why-straight-line-radius-geofencing-fails-direct-answer-to-the-railway-line-case)
6. [How Isochrones Are Actually Computed](#6-how-isochrones-are-actually-computed)
7. [Point-in-Polygon & Spatial Indexing at Scale](#7-point-in-polygon--spatial-indexing-at-scale)
8. [Multi-Store Overlap & Assignment](#8-multi-store-overlap--assignment)
9. [Dynamic Adjustment & Degraded Conditions](#9-dynamic-adjustment--degraded-conditions)
10. [Factors You Didn't Mention — But That Matter](#10-factors-you-didnt-mention--but-that-matter)
11. [Constraints That Shape This System](#11-constraints-that-shape-this-system)
12. [Base Assumptions](#12-base-assumptions)
13. [Guesstimates Cheat Sheet](#13-guesstimates-cheat-sheet)

---

## 1. Reframing the Problem

The instinct to reach for "draw a circle of radius X around the store" is the single most common mistake in this problem — and you've already spotted exactly why it breaks, with the railway-line example. The correct framing is:

> **A dark store's serviceable area is not a geometric shape centered on a point. It's the set of locations reachable within a time budget, travelling along the actual road network, in the actual direction of travel, by the actual vehicle used for delivery.**

This reframes the problem from **computational geometry** (circles, radii) to a blend of three distinct sub-problems:

1. **Graph/network science** — compute "how far can I actually get in 15 minutes from this point" on a real road graph (this is called an **isochrone** or **drive-time/service-area polygon** in GIS literature, and it's the direct, principled answer to your railway-line question).
2. **Computational geometry** — once you have that oddly-shaped, possibly non-convex polygon (with "bites" taken out of it by barriers), determine whether a given user's location falls inside it. This is the classic **point-in-polygon problem**.
3. **Spatial indexing & systems engineering** — doing step 2 for millions of app opens per day cheaply, which in practice means *not* doing live polygon math per request at all, but something smarter (Section 7).

Additionally, there's a fourth layer that's easy to miss: once you have a correct isochrone, **multiple stores' isochrones overlap** in dense cities, and **capacity constraints** (inventory, rider availability) mean geographic reachability alone doesn't guarantee serviceability at any given moment. Both are covered below.

---

## 2. Data You Need

| Category | Specifics |
|---|---|
| **Road network graph** | Nodes = intersections, edges = road segments with length, speed limit/typical speed, one-way direction, turn restrictions — ideally mode-specific for two-wheelers (can use lanes/shortcuts a car cannot) |
| **Barrier/infrastructure data** | Explicit mapping of railway crossings, bridges, flyovers, underpasses, rivers/canals, gated community access points — this is the data that *makes or breaks* accuracy on exactly the case you raised |
| **Historical delivery GPS traces** | Actual completed delivery trips (store → customer), with real travel time and path taken — this is ground truth to calibrate assumed speeds and validate computed isochrones against reality |
| **Rider/vehicle speed profiles** | Average real-world speed by road type and by zone (dense market lanes vs. wide arterial roads), ideally derived from your own fleet's GPS rather than generic assumptions |
| **Store location & capacity data** | Precise store coordinates, current inventory depth, number of active riders, current order queue/load |
| **Customer address/geocoding data** | Pin accuracy, especially for apartment complexes and informal addressing — a major real-world source of error, independent of the algorithm's correctness |
| **Demand density data** | App opens, searches, and orders by location — both inside and *outside* current coverage, since "failed/blocked" demand at the edges is a strong signal for future store placement |
| **Real-time traffic data** | For dynamic/time-of-day adjustment of travel-time assumptions |
| **Regulatory/access restriction data** | Two-wheeler no-entry zones, time-of-day vehicle restrictions, one-ways that differ for different vehicle classes |

---

## 3. Questions to Ask If This Data Doesn't Exist Yet

- **"Is our road network graph self-hosted (OSM-based via OSRM/Valhalla/pgRouting) or a licensed third-party service, and has anyone audited it for missing or wrong barrier crossings in our serviceable cities?"**
  This is the single highest-leverage question. Crowd-sourced map data (OpenStreetMap) is usually good on major roads but frequently wrong or missing on exactly the things that matter here — a newly built pedestrian bridge, a railway underpass that's two-wheeler-only, a gated community with a single functioning gate. Garbage-in on barrier data produces a wrong isochrone no matter how good the algorithm is.

- **"Do we validate computed isochrones against actual completed delivery GPS traces, or do we trust the routing engine's theoretical output?"**
  Without this feedback loop, you won't catch cases where the map says a route is open but it's actually a locked gate, a one-way restriction the map missed, or a shortcut riders use that the map doesn't know about.

- **"What exactly does our '10-20 minutes' threshold represent — pure ride time, or does it include a buffer for picking/packing and handover at the door?"**
  If the threshold is being applied to raw ride time only, real-world SLA breaches will be systematically undercounted in the isochrone math.

- **"Is serviceability precomputed into static zones refreshed periodically, or checked live via a routing API call at the moment a customer opens the app / places an order?"**
  Both approaches exist in production (and the right answer is usually a hybrid — see Section 7) — but this determines your cost structure and how fast the system reacts to road changes.

- **"Do we have a process for ops teams to manually override or patch a computed zone?"**
  Automated routing data is rarely perfect at the hyper-local level (a single-gate apartment complex, a lane blocked by informal construction) — most real quick-commerce operators layer manual ground-truth corrections on top of the automated isochrone rather than trusting it blindly.

---

## 4. System Architecture: The Geofencing Pipeline

```
 ┌─────────────────────────────────┐
 │     Road Network Graph            │  Nodes, edges, speeds,
 │                                   │  barrier crossings
 └─────────────────┬───────────────────┘
                   │
 ┌─────────────────▼───────────────────┐
 │   Isochrone Generation              │  Time-bounded multi-source
 │                                   │  shortest-path search
 └─────────────────┬───────────────────┘
                   │
 ┌─────────────────▼───────────────────┐
 │   Spatial Index Build               │  Polygon → hex grid
 │                                   │  (e.g. H3) cell set
 └─────────────────┬───────────────────┘
                   │
 ┌─────────────────▼───────────────────┐
 │  Serviceability Lookup (runtime)    │  O(1) cell membership
 │                                   │  check per app request
 └─────────────────┬───────────────────┘
                   │
 ┌─────────────────▼───────────────────┐
 │   Multi-Store Overlap Resolution    │  Best-store assignment
 │                                   │  when zones overlap
 └─────────────────┬───────────────────┘
                   │
 ┌─────────────────▼───────────────────┐
 │  Capacity & Dynamic Throttling      │  Inventory/rider load,
 │                                   │  weather, traffic
 └─────────────────┬───────────────────┘
                   │
 ┌─────────────────▼───────────────────┐
 │       App Response                  │  "Deliverable in X min"
 │                                   │  or "Not serviceable"
 └──────────────────────────────────────┘
                   ▲
                   │
 ┌─────────────────┴───────────────────┐
 │  Ground-Truth Feedback Loop         │  Actual delivery GPS traces
 │                                   │  validate/correct isochrones
 └──────────────────────────────────────┘
```

The top three boxes run as a **periodic batch job** (hours to weeks, not per-request). The middle three run **live, per app open/order**. The feedback loop closes the system — it's what catches the cases where the map was wrong about a barrier crossing.

---

## 5. Why Straight-Line Radius Geofencing Fails (Direct Answer to the Railway-Line Case)

Walking through your exact example: Store at point B, customer at point A, straight-line distance 300m, but the only actual route is an 8-9km detour over a bridge because a railway line runs between them with no nearby crossing.

**A circular/radius-based geofence** (e.g., "serviceable if within 2.5km straight-line") would **incorrectly mark A as serviceable** — it has no concept of the railway line at all; it's pure Euclidean geometry on a map that, in reality, is not uniformly traversable in every direction.

**A network-based isochrone** gets this right *automatically*, with no special-case logic needed:

1. The underlying road graph used to generate the isochrone only has edges where real roads/paths exist. Between A and B, if the only connecting edges go via the 8-9km bridge route, that is reflected directly in the graph's structure.
2. When the system runs a time-bounded shortest-path search outward from B (Section 6), it reaches A only after traversing that long bridge route — accumulating, say, 20-25+ minutes of travel time at typical speeds.
3. Since that exceeds the 10-20 minute threshold, **A is simply excluded from B's isochrone polygon** — not because of a special "railway line" rule, but as a natural consequence of correctly modeling the road network and running a shortest-path search on it.

This is the core insight: **you don't need to hand-code exceptions for every bridge, river, or gated community. You need an accurate graph of what's actually connected to what, and the shortest-path math handles the rest.** The entire burden of correctness shifts to *map/graph data quality* — which is exactly why Section 2's barrier-infrastructure data and Section 9's ground-truth feedback loop matter so much in practice. The algorithm is conceptually simple; getting the underlying road graph right (especially informal shortcuts, gated-community gates, and newly built crossings) is where the real engineering effort goes.

---

## 6. How Isochrones Are Actually Computed

**Step 1 — Snap the store to the road graph.** The dark store's coordinates are matched to the nearest routable node/edge on the road network (the store itself isn't "on" the graph; its access point, like a gate or road frontage, is).

**Step 2 — Run a time-bounded multi-source shortest-path search.** Starting from the store's graph node, run a variant of Dijkstra's algorithm (or a label-correcting search) where edge weights are **travel time**, not distance — computed from segment length and a realistic speed-per-road-type for the delivery vehicle (two-wheeler speeds differ meaningfully from car speeds, especially in dense lanes). Expand outward from the store, accumulating travel time along each path, and stop expanding once the cumulative time exceeds the threshold (say, 18 minutes: 15 minutes riding + a ~3 minute buffer for parking/handover).

This produces a **set of reachable road-network nodes**, each tagged with its shortest travel time from the store — exactly analogous to the time-expanded routing concept used in inter-city logistics, just applied at street-network granularity instead of hub-to-hub.

**Step 3 — Build the polygon from the reachable set.** The reachable nodes, when enclosed, do *not* form a circle or even a convex shape — they form an irregular, often "star-shaped" or "bitten" polygon that hugs the actual road network and excludes pockets cut off by barriers (your railway-line case shows up here as a literal bite taken out of the shape). Two common techniques:

- **Concave hull / alpha shape** around the reachable nodes — tighter and more accurate than a convex hull, which would incorrectly smooth over the barrier-caused gaps.
- **Buffer-and-union** — draw a small buffer polygon around each reachable road segment and union them all together, which naturally traces the road network's actual shape.

This is the same technique GIS tools call **"service area analysis"** (ArcGIS Network Analyst, pgRouting's drive-time polygons, or open-source isochrone APIs built on OSRM/Valhalla) — not a novel algorithm, but a well-established one worth knowing by name.

**Step 4 — Account for directionality.** Because of one-way streets, the time to travel *from* the store *to* a point can differ from the reverse direction. Since quick commerce only cares about the outbound (store → customer) leg, the search must be run in the **forward direction from the store**, not as an undirected/symmetric search — otherwise the isochrone can be subtly wrong on roads with meaningful one-way structure.

---

## 7. Point-in-Polygon & Spatial Indexing at Scale

### 7.1 The classic algorithm

Once you have a polygon (the isochrone from Section 6), determining whether a customer's coordinate falls inside it is the textbook **point-in-polygon problem**. The standard approach is the **ray casting / crossing-number algorithm**: draw a ray from the point out to infinity in any fixed direction, and count how many polygon edges it crosses — an **odd** number of crossings means the point is inside, an **even** number means outside. (The **winding number algorithm** is a more robust variant that also handles self-intersecting polygons correctly.) In practice, nobody hand-rolls this — spatial libraries (Shapely in Python, PostGIS's `ST_Contains`/`ST_Within` in a database) implement it efficiently, often backed by an **R-tree spatial index** so that a point isn't even tested against polygons whose bounding box can't possibly contain it.

### 7.2 Why real systems don't actually do live polygon math per request

Running polygon containment tests against a database of store-shaped polygons, for every single app open, across millions of daily users, is more computation than necessary. What large-scale systems (Uber, and quick-commerce platforms generally) actually do is convert the problem into **spatial hashing**:

1. **Tessellate the map into a fixed grid of cells** — the industry-standard tool for this is **Uber's H3** (a hexagonal hierarchical spatial index), though a simpler geohash grid works too. Hexagons are preferred over squares because they have uniform adjacency (every neighbor is equidistant), which avoids distortion in later distance-based logic.
2. **Precompute, once per store (and periodically refresh), the full set of grid cells that fall inside that store's isochrone polygon.** This is a one-time (well, periodic) conversion from "polygon" to "a set of cell IDs" — store this as a simple set/hash table, e.g., in Redis: `store_id → {cell_1, cell_2, ..., cell_n}`, or inverted as `cell_id → [store_id_1, store_id_2, ...]` for even faster lookup.
3. **At request time**, convert the customer's lat/long into its corresponding grid cell ID (a deterministic, extremely fast calculation — no polygon math at all), and do an **O(1) set-membership lookup** against the relevant store(s)' precomputed cell sets.

This turns an expensive geometry problem into a cache lookup, which is the only way this scales to the request volumes a consumer app sees. The resolution of the grid (roughly how large each hex cell is) is a tunable trade-off: finer cells trace the actual isochrone boundary more precisely but mean more cells to store and more boundary cells needing careful handling; coarser cells are cheaper but risk including/excluding the wrong areas near the polygon's edge.

### 7.3 Boundary fuzziness — handling the edge case correctly

No matter how fine the grid, there will be customers right at the boundary where a cell is only partially inside the true isochrone. Two practical responses:

- **Conservative bias**: when a cell straddles the boundary, default to "not serviceable" rather than "serviceable" — overpromising and failing an SLA is reputationally worse than underpromising.
- **Hybrid live check at final checkout**: use the fast grid lookup for the general "do we deliver here" indicator shown broadly in the app, but for addresses that fall in a boundary cell specifically, trigger one live routing-API call at actual order placement to get a precise answer — spending the expensive computation only where cheap computation is genuinely ambiguous.

---

## 8. Multi-Store Overlap & Assignment

In dense cities, it's common for a customer's location to fall inside **more than one store's isochrone** simultaneously. When that happens, which store serves the order? This is a small assignment decision, not a geometry question, and should weigh:

- **Predicted delivery time** from each candidate store (not just "inside the zone," but *how deep* inside — a point 3 minutes from one store and 14 minutes from another, both technically "serviceable," should clearly prefer the former)
- **Current inventory availability** for the specific items in the cart — a geographically closer store that's stocked out of half the basket is a worse choice than a slightly farther one that has everything
- **Current store load** (queue length, rider availability) — load balancing across overlapping stores avoids overwhelming one while another sits idle
- **Business rules**, such as deliberately routing some volume to a newly opened store to help it ramp up, even if a nearby established store is marginally faster

This is conceptually the same "pick the best option among multiple feasible candidates" logic used in rider-dispatch matching — just applied at the store-selection layer instead of the rider layer.

---

## 9. Dynamic Adjustment & Degraded Conditions

An isochrone computed once under ideal conditions will drift out of sync with reality over time and across conditions. A mature system treats serviceability as **periodically refreshed, not permanently fixed**:

- **Time-of-day variation.** Peak-hour traffic can meaningfully shrink the realistically reachable area compared to off-peak; some systems maintain two or three isochrone variants (peak/off-peak) rather than a single static zone, or apply a traffic-derived scaling factor to the base isochrone.
- **Weather-driven shrinkage.** Heavy rain measurably reduces safe/achievable two-wheeler speed — the serviceable polygon should contract (and SLA promises adjust) rather than staying fixed while real delivery times blow out underneath it.
- **Capacity-driven dynamic unserviceability.** A location can be geographically inside the isochrone and still be *operationally* unserviceable right now if the store has no available riders or is out of stock on relevant items — this is a runtime check layered on top of the static geofence, not a replacement for it.
- **Periodic ground-truth re-validation.** Compare the isochrone's predictions against actual completed delivery GPS traces on a regular cadence (e.g., monthly, or continuously as a monitoring metric) — if real deliveries to a supposedly "outside the zone" address keep succeeding well within SLA, or supposedly "inside the zone" addresses keep breaching SLA, that's a signal the underlying road graph or speed assumptions need correction.
- **Infrastructure change triggers.** New roads, bridges, or metro lines materially change connectivity — isochrones should be regenerated when the underlying map data updates, not left stale indefinitely.

---

## 10. Factors You Didn't Mention — But That Matter

- **Manual ops override layer.** No automated road-graph data is perfect at the hyper-local level — a gated community with one functioning gate, an informally closed lane, a newly built pedestrian-only shortcut riders actually use. Most real-world systems let local ops teams manually patch/adjust a computed zone rather than trusting the algorithm blindly end-to-end.
- **Address/geocoding error as an independent failure mode.** Even a perfect isochrone fails if the customer's pin is dropped at the wrong location (common for large apartment complexes with one entrance far from where the pin lands) — this is a data-quality problem entirely separate from the routing logic, but it produces identical-looking symptoms ("wrongly marked unserviceable/serviceable").
- **Dead-zone detection feeding site selection.** Areas that fall outside every existing store's isochrone, especially ones showing meaningful app-open or search demand, are a direct signal for where to place the *next* dark store — this turns the geofencing system into a live input for expansion planning (a maximum-covering-location-style optimization: where should new stores go to cover the most currently-unserved demand), not just a static operational tool.
- **Vehicle-class-specific isochrones.** If the fleet includes both two-wheelers and bicycles/on-foot couriers for very short hops, each effectively has a different reachable area and speed profile — a single one-size-fits-all isochrone can understate reach for faster vehicles or overstate it for slower ones.
- **Cold-chain/perishable sub-zones.** Frozen or ice-cream items may need a tighter time threshold than the general catalog — practically, this can mean a smaller "nested" isochrone (e.g., 10-minute polygon) used specifically to gate which orders are allowed to include those SKUs, inside the broader general-delivery polygon.
- **Redundancy/overlap planning, not just overlap resolution.** Deliberately engineering some overlap between neighboring stores' zones (rather than treating overlap as purely incidental) provides resilience — if one store goes down (stockout, no riders, closure), neighboring coverage can absorb some of its load instead of leaving a hard gap.
- **Regulatory/vehicle-class road restrictions.** Some roads have time-of-day or vehicle-class restrictions (e.g., two-wheeler no-entry during certain hours, heavy-vehicle-only lanes) that a generic routing engine's default profile may not encode correctly — worth explicitly auditing for the delivery vehicle class actually used.
- **New store cannibalization modeling.** When a new dark store's isochrone is being planned, its overlap with existing stores' zones affects not just "who serves this customer" but existing stores' order volume and economics — this is a factor in *where* to place new stores, not only in how to assign orders once placed.

---

## 11. Constraints That Shape This System

| Constraint Type | Specifics |
|---|---|
| **Map/graph data quality** | The entire correctness of the system depends on an accurate road network including barrier crossings — this is the dominant source of real-world error, more so than algorithmic sophistication |
| **Computational cost at scale** | Live polygon math per request doesn't scale to consumer app volumes — necessitates precomputed spatial-grid indexing (Section 7) |
| **Directionality** | One-way streets make travel time asymmetric — isochrones must be computed in the correct (outbound) direction, not as an undirected approximation |
| **Freshness vs. cost of recomputation** | Isochrones drift out of date as roads/traffic change, but recomputing continuously for every store is expensive — needs a defined refresh cadence |
| **Capacity (inventory/rider availability)** | Geographic reachability is necessary but not sufficient for serviceability — a store can be "in range" but operationally unable to fulfill right now |
| **SLA commitment (10-20 min)** | A hard business constraint the threshold itself derives from, and one that should include a handover/buffer margin, not just pure ride time |
| **Vehicle capability & regulation** | Two-wheeler-specific routing profiles, time-of-day access restrictions, and road-class rules all constrain which paths are legitimately usable |
| **Address/geocoding accuracy** | Independent of the routing algorithm's correctness, bad pins produce the same visible symptom (wrong serviceability determination) |
| **Boundary ambiguity** | Grid-cell resolution trade-offs mean some locations near the true polygon edge are inherently ambiguous and need an explicit conservative-default or live-check policy |
| **Multi-store coordination** | Overlapping zones require an assignment policy beyond pure geometry — load, inventory, and ramp-up business rules all factor in |

---

## 12. Base Assumptions

1. The primary delivery vehicle is a two-wheeler in a dense urban Indian (or similarly structured) city, where barriers like railway lines, rivers, and gated communities materially affect connectivity — this shapes the emphasis on network-based rather than radius-based geofencing.
2. A road network graph (self-hosted via OSM/OSRM/Valhalla, or licensed) already exists or is readily obtainable — building a road graph entirely from scratch would be a larger separate effort than assumed here.
3. The 10-20 minute threshold is a given business/SLA constraint, not something this system needs to derive — though the document assumes it should include a buffer for handover, not just pure ride time.
4. Store locations and basic operational data (inventory, rider count) already exist in some operational system this geofencing logic can query.
5. The company operates (or plans to operate) multiple stores with geographically overlapping or near-overlapping catchments in at least some cities — if store density is very sparse with zero overlap anywhere, Section 8's assignment logic simplifies away.

---

## 13. Guesstimates Cheat Sheet

Rough, directional numbers to reason with — not precise constants, and worth calibrating against your own fleet's actual GPS data as soon as it's available:

| Quantity | Rough Guesstimate |
|---|---|
| Two-wheeler real-world urban speed (with traffic/stops, dense Indian city) | ~15-20 km/h, vs. ~30-40 km/h theoretical free-flow |
| Handover/buffer time (parking, building access, OTP confirmation) | ~2-3 minutes, on top of pure ride time |
| Effective "reach" for a 15-minute ride budget (after buffer) at ~15-18 km/h | Roughly 3-4.5 km of actual road distance — note this is *road* distance, which in a barrier-heavy area can correspond to a much smaller straight-line radius |
| Isochrone shape vs. a circle of equivalent area | Typically irregular/star-shaped with real-world "bites," never a clean circle, in any city with rivers, rail lines, or limited-access highways |
| Spatial grid cell size for geofence lookups (e.g., H3 resolution) | City-block-level granularity, roughly 150-500 meters across, balances boundary precision against the number of cells to store and query |
| Isochrone recomputation cadence | Days to a few weeks for routine refresh; immediately on known infrastructure changes (new bridge, road closure); continuous lightweight monitoring via delivery-outcome feedback in between |
| Overlap tolerance between neighboring stores | A deliberate small overlap (rather than zero or large overlap) is common — enough for resilience/load-balancing without excessive order-splitting ambiguity |

---

*Document prepared as a system design framework for dark store geofencing, isochrone-based serviceability determination, and road-network-aware catchment definition in quick commerce.*