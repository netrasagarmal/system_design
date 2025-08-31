### 1. **Vertical and Horizontal Scaling**

| **Vertical Scaling (Scale-Up)** | **Horizontal Scaling (Scale-Out)** |
| --- | --- |
| Add more power (CPU, RAM) to a single server | Add more servers to distribute load |
| Limited by hardware | Scales better with demand |
| Simpler to implement | More complex (needs load balancing) |
| Example: Upgrading server from 8 GB to 32 GB RAM | Example: Adding more servers to handle web traffic |

---

### 2. **Load Balancing**

- **Purpose**: Distribute incoming traffic across multiple servers to ensure no single server gets overwhelmed.
- **Types**:
    - **Round Robin**: Evenly distributes requests.
    - **Least Connections**: Sends to the server with the fewest active connections.
    - **IP Hash**: Based on client's IP for sticky sessions.
- **Example**: A web app with 3 backend servers behind a load balancer. It ensures users are sent to any one of them based on rules.

---

### 3. **Message Queue vs Event Bus**

| **Message Queue** | **Event Bus** |
| --- | --- |
| Point-to-point communication (1 producer, 1 consumer) | Pub/Sub model (1 event can go to many consumers) |
| Maintains strict ordering | Focuses on broadcasting events |
| Examples: RabbitMQ, Amazon SQS | Examples: Kafka, NATS, EventBridge |
| Use-case: Order processing pipeline | Use-case: Triggering multiple microservices on user signup |

---

### 4. **Publisher-Subscriber Model**

- **Definition**: Publishers send messages (events), and subscribers listen to specific topics or event types.
- **Decoupling**: Publisher doesn’t know who subscribers are.
- **Example**:
    - Publisher: A payment service emits `payment_success` event.
    - Subscribers:
        - Email service sends receipt.
        - Inventory service updates stock.
        - Analytics service logs transaction.

---

### 5. **Monolithic vs Microservices**

| **Monolithic Architecture** | **Microservices Architecture** |
| --- | --- |
| All features in a single deployable unit | Features split into independent services |
| Simple to develop initially | Better scalability and fault isolation |
| Harder to maintain as it grows | More complex infrastructure |
| Example: An e-commerce app with all features in one server | Example: Separate services for cart, payment, orders |

---

### 6. **Caching (Server-side vs Client-side)**

| **Server-side Caching** | **Client-side Caching** |
| --- | --- |
| Cache responses on server (Redis, Memcached) | Browser or app caches data locally |
| Reduces DB hits and speeds up APIs | Reduces network calls |
| Example: Cache user session tokens | Example: Caching images, static files in browser |

---

### 7. **Content Delivery System (CDN)**

- **Purpose**: Distribute static content closer to the user to reduce latency.
- **Works by**:
    - Caching assets like JS, CSS, images at edge locations.
    - Serving based on nearest geographical location.
- **Examples**: Cloudflare, Akamai, AWS CloudFront
- **Use-case**: Hosting website images, fonts, and scripts to speed up page loads.

---

### 8. **Event-Driven System Architecture**

- **Definition**: Systems communicate through events.
- **Decouples** producers and consumers.
- **Example Workflow**:
    - User uploads image → Event fired → Services listen:
        - One service resizes image
        - Another updates user feed
        - Another logs audit trail
- **Tools**: Kafka, RabbitMQ, AWS SNS/SQS, Azure EventGrid

---

### 9. **Client-Server Architecture**

- **Definition**: A model where clients (browsers/apps) request data/services from a centralized server.
- **Example**:
    - Client: Web browser
    - Server: REST API serving data from DB
- **Flow**:
    
    ```
    Client → Request → Server
            ← Response ←
    
    ```
    

---

### 10. **Peer-to-Peer (P2P) Architecture**

- **Definition**: No centralized server, all nodes (peers) share and request data from each other.
- **Example**:
    - BitTorrent: Each user downloads and uploads file parts.
- **Pros**:
    - Scalable, resilient
- **Cons**:
    - Hard to manage/control

---

### 11. **API Gateway Working and Routing**

- **Acts as the single entry point** for all client requests to microservices.
- **Responsibilities**:
    - Authentication/Authorization
    - Rate limiting
    - Request routing
    - Aggregating responses
- **Example**:
    - Client sends `/user/123` → API Gateway → Routes to User Service
    - Client sends `/order/456` → Routes to Order Service
- **Tools**: Kong, AWS API Gateway, NGINX, Apigee

---

### 🔍 **What is CDC?**

CDC detects data changes in a **source system** (usually a database) and delivers them to **target systems** (like data warehouses, caches, search engines, microservices, etc.) without needing to scan the entire database.

---

### ⚙️ **How CDC Works (Mechanisms)**

There are three main ways to implement CDC:

### 1. **Log-Based CDC (Most Efficient)**

- Reads the database’s **transaction log (binlog, WAL, redo log)** to capture changes.
- **No impact on production DB performance**.
- Works with tools like Debezium, AWS DMS.

### 2. **Trigger-Based CDC**

- Uses **triggers in the DB** to track changes in real-time.
- Inserts change data into an audit table.
- More intrusive and adds overhead.

### 3. **Table Diff / Polling (Least Efficient)**

- Periodically polls and compares snapshots of tables.
- High latency and DB load.

---

### 📦 **Types of Changes Captured**

Each change is usually captured with:

- **Change type**: INSERT, UPDATE, DELETE
- **Before and/or After** image of the data
- **Timestamp**
- **Primary key or identifier**

---

### 💡 **Example: Log-Based CDC in an E-Commerce App**

Let’s say you have a PostgreSQL database with an `orders` table:

```sql
orders(id, user_id, status, total_price, updated_at)

```

A user places an order → an `INSERT` happens.

### 🔄 CDC Flow:

1. The **PostgreSQL WAL log** records the change.
2. **Debezium** (CDC connector) reads the log and emits a change event.
3. The event looks like:

```json
{
  "operation": "insert",
  "table": "orders",
  "data": {
    "id": 101,
    "user_id": 55,
    "status": "PLACED",
    "total_price": 299.99,
    "updated_at": "2025-06-13T14:01:22Z"
  }
}

```

1. The event is **pushed to Kafka**.
2. Downstream services:
    - Search index updates to include new order.
    - Analytics pipeline processes it for real-time dashboards.
    - Inventory service deducts stock.
    - Notification service sends email.

---

### 🔧 **Popular CDC Tools**

| Tool | Notes |
| --- | --- |
| **Debezium** | Open-source CDC platform supporting MySQL, Postgres, MongoDB, etc. |
| **AWS DMS** | Managed CDC + data replication |
| **Apache Kafka Connect** | With CDC connectors (e.g., Debezium) |
| **Oracle GoldenGate** | Enterprise CDC for Oracle DBs |
| **SQL Server CDC** | Built-in feature in SQL Server |

---

### ✅ **Use Cases of CDC**

- Real-time data pipelines (ETL)
- Microservices that need DB event updates
- Syncing data between different systems
- Caching (e.g., updating Redis on DB change)
- Auditing or compliance systems

---

### ⚠️ **Challenges**

- Handling schema changes
- Ensuring **exactly-once** delivery
- Event ordering in distributed systems
- Backpressure if downstream is slow

---

Let me know if you want a **visual architecture diagram** of CDC in action or a working demo using tools like Debezium + Kafka.

### Kubernetes

Here's a **detailed breakdown of Kubernetes concepts**, architecture, and how it manages load balancing and IP routing, with a focus on **AKS (Azure Kubernetes Service)** where applicable.

---

## 🧠 **1. What is Kubernetes?**

Kubernetes (aka **K8s**) is an **open-source container orchestration platform** for automating deployment, scaling, and management of containerized applications (like Docker containers).

---

## 🧩 **2. Kubernetes Cluster**

A **Kubernetes cluster** is made of:

- **Master Node(s)** (Control Plane): Manages the cluster.
- **Worker Nodes** (Minions): Where your containerized apps run.
- Think of it like a brain (**Control Plane**) and muscles (**Worker Nodes**).

---

## 🧱 **3. Master Node / Control Plane Components**

Master node **controls and schedules** all other nodes and workloads.

### Key Components:

| Component | Description |
| --- | --- |
| **API Server** | Front door of the cluster; handles requests (kubectl, dashboard, etc.). |
| **Scheduler** | Assigns Pods to available nodes based on resource needs. |
| **Controller Manager** | Handles background tasks like node monitoring, replicating pods, etc. |
| **etcd** | Key-value store (database) that stores the state/configuration of the cluster. |
| **Cloud Controller Manager (in AKS)** | Interfaces with Azure-specific resources like LB, disks, etc. |

---

## 💼 **4. AKS Node Pool**

- In **Azure Kubernetes Service (AKS)**:
    - **Node Pool** = Group of worker nodes with the same VM type.
    - You can have multiple pools (e.g., one for general apps, one for GPU-intensive ML apps).
- Example:
    
    ```bash
    az aks nodepool add --name mlpool --node-vm-size Standard_NC6 --node-count 2
    
    ```
    

---

## 🧑‍🏭 **5. Worker Node**

Each **worker node** runs:

- **kubelet**: Communicates with the API server.
- **kube-proxy**: Handles networking/routing.
- **Container Runtime**: Docker, containerd, etc., to run containers.
- One node can run multiple **Pods**.

---

## 📦 **6. Pod**

- Smallest **deployable unit** in Kubernetes.
- A **Pod** wraps one or more containers **that share**:
    - Storage (Volumes)
    - Network (one IP per pod)
    - Lifecycle

> Example: A pod might run an app container and a logging sidecar container.
> 

---

## 📡 **7. Kubernetes API Server**

- Acts as the **REST API frontend** to the entire cluster.
- Everything (kubectl, Helm, dashboard, CI/CD) talks to the cluster via the **API Server**.
- Validates requests, updates `etcd`, and triggers controllers.

---

## 🧭 **8. Kubernetes Load Balancing**

Kubernetes supports **load balancing** at different levels:

### A. **Pod-Level (Internal)**

- Uses **Services** with selectors to forward traffic to Pods.
- **Types**:
    - `ClusterIP`: Default, internal-only access.
    - `NodePort`: Exposes service on a static port on each node.
    - `LoadBalancer`: Provisions cloud LB (in AKS, Azure Load Balancer).

> Example:
> 

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 8080

```

### B. **Ingress (HTTP Routing)**

- Acts as an **L7 reverse proxy**.
- Supports:
    - Path-based routing (`/api`, `/auth`)
    - Host-based routing (`foo.example.com`, `bar.example.com`)
- Backed by **Ingress Controller** like NGINX, Traefik, AGIC (Azure).

---

## 🔀 **9. IP Routing in Kubernetes**

Each pod gets its own **IP address** within a virtual network.

### Networking Model:

- **Pod-to-Pod communication** works **across nodes without NAT**.
- Achieved using a **CNI plugin** (Container Network Interface).
    - AKS supports **Azure CNI**, Kubenet, and more.

### IP Flow Example:

```
User hits LoadBalancer IP → Service routes to a Pod → kube-proxy/IPTables rules handle routing → Container receives request

```

### kube-proxy Role:

- On every node.
- Manages **iptables rules** or **IPVS** to route traffic to the correct pod.
- Load balances requests between matching pods.

---

## 🧭 **How Traffic Flows (Full Picture)**

1. **User** accesses a service (say via IP or DNS).
2. Traffic hits the **Azure Load Balancer** (if Service type is LoadBalancer).
3. Load balancer routes to a **NodePort** on one of the worker nodes.
4. **kube-proxy** on that node chooses a Pod from the matching set.
5. Traffic reaches the container in the Pod.

---

## 🔄 **How Scaling Works**

- Kubernetes supports:
    - **Horizontal Pod Autoscaler (HPA)**: Scales pods based on CPU/memory.
    - **Cluster Autoscaler**: Adds/removes nodes in a node pool.
- In AKS, Azure provisions more VMs when needed.

---

## 📌 Summary Table

| Component | Role |
| --- | --- |
| **Cluster** | Group of master + worker nodes |
| **Master Node / Control Plane** | Manages and schedules workloads |
| **Worker Node** | Runs your applications (pods) |
| **Pod** | Smallest unit with one/more containers |
| **Node Pool (AKS)** | VM group for scaling workloads |
| **Service** | Exposes Pods internally or externally |
| **Ingress** | HTTP/HTTPS routing (path/host-based) |
| **API Server** | Entry point for commands and requests |
| **kube-proxy** | Handles internal routing and load balancing |

---

Would you like:

- A **diagram** of this architecture?
- A real **kubectl command flow** example?
- A **sample deployment.yaml + service.yaml**?

Let me know!

### Deployment stratergies

Here’s a **detailed explanation of major deployment strategies**, with **use cases, pros/cons, and examples**. These are crucial in **DevOps and system design** to ensure smooth, safe, and zero-downtime deployments.

---

## 🚀 **1. Blue-Green Deployment**

### 📌 **What It Is**

- Maintain **two environments**:
    - **Blue** (current/production)
    - **Green** (new version)
- Switch traffic to **Green** after testing.

### ✅ Pros:

- Near-zero downtime
- Easy rollback (just switch back to Blue)
- Test in production-like environment

### ❌ Cons:

- Double infrastructure cost
- Complex setup if there are stateful components (DBs)

### 🔧 Example:

- Blue: v1.0 running on `prod.myapp.com`
- Deploy v2.0 to Green (on standby)
- After verification, change router/load balancer to point to Green

---

## 🐦 **2. Canary Deployment**

### 📌 **What It Is**

- Deploy new version to a **small percentage of users** first
- Gradually increase the percentage over time

### ✅ Pros:

- Controlled risk
- Real-user feedback before full rollout
- Easier to monitor errors early

### ❌ Cons:

- Needs monitoring, routing control, and automation
- More complex than rolling updates

### 🔧 Example:

- v1.0 serves 95%, v2.0 serves 5% of traffic
- If successful, move to 25% → 50% → 100%

**Used by:** Netflix, Google, Facebook

---

## 🔄 **3. Rolling Deployment**

### 📌 **What It Is**

- Replace old version with new version **incrementally**, pod by pod or server by server

### ✅ Pros:

- No downtime
- Minimal infrastructure overhead
- Standard in Kubernetes (`rollingUpdate` strategy)

### ❌ Cons:

- Rollback is slow (must roll back each unit)
- Might lead to inconsistent behavior if clients hit different versions

### 🔧 Example in Kubernetes:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1

```

---

## 💥 **4. Big Bang Deployment**

### 📌 **What It Is**

- **Entire application** or system is replaced with the new version **at once**

### ✅ Pros:

- Simple to implement (no need for multiple environments)

### ❌ Cons:

- **High risk**: Any bug affects all users
- No real-time rollback
- Downtime likely

### 🔧 Use-case:

- Small teams
- Internal tools
- MVP launches

---

## 🕵️ **5. Shadow Deployment**

### 📌 **What It Is**

- New version runs in parallel but **does not receive live traffic** from users
- It **mirrors real user traffic** in the background for testing

### ✅ Pros:

- Test with production load safely
- Detect performance or logic issues early

### ❌ Cons:

- Complex setup (duplicate routing, load handling)
- Can’t test user impact (since no real user sees it)

### 🔧 Example:

- Requests to v1.0 are also copied to v2.0 silently (for testing responses or performance)

---

## 🧨 **6. Recreate Deployment**

### 📌 **What It Is**

- **Shut down** the old version completely, then start the new one

### ✅ Pros:

- Simple and easy
- Clean state

### ❌ Cons:

- **Downtime required**
- Not ideal for critical services

### 🔧 Kubernetes Example:

```yaml
strategy:
  type: Recreate

```

---

## 🧭 **Summary Comparison Table**

| Strategy | Zero Downtime | Rollback Ease | Complexity | Best For |
| --- | --- | --- | --- | --- |
| Blue-Green | ✅ | ✅ | Medium | Fast rollback |
| Canary | ✅ | ✅ | High | Risk mitigation |
| Rolling | ✅ | Medium | Low | Regular updates |
| Big Bang | ❌ | ❌ | Low | Small projects |
| Shadow | ✅ | ✅ | High | Testing in prod |
| Recreate | ❌ | ✅ | Low | Non-critical apps |

---

Would you like:

- Diagrams for each strategy?
- Real-world CI/CD YAML examples (like GitHub Actions or ArgoCD)?
- How to implement these in Kubernetes, Azure DevOps, or AWS?

Let me know!

### Ways of scaling a system

Scaling a system is about increasing its ability to handle more **load**, **users**, **requests**, or **data** efficiently. Below are the **major types and strategies of system scaling**, with explanations, examples, and when to use each.

---

## 🧱 1. **Vertical Scaling (Scale-Up)**

### 🔹 What It Is:

- Add **more resources** (CPU, RAM, SSD, etc.) to an existing server/machine.

### 📌 Example:

- Upgrade a server from 8GB RAM to 64GB, or 4 CPUs to 32 CPUs.

### ✅ Pros:

- Simpler architecture
- Less code/config change

### ❌ Cons:

- **Hardware limits**
- Single point of failure
- More expensive at scale

---

## 🧱 2. **Horizontal Scaling (Scale-Out)**

### 🔹 What It Is:

- Add **more machines**/nodes to distribute load.

### 📌 Example:

- Add more app servers behind a **load balancer**

### ✅ Pros:

- Handles large scale easily
- High availability & redundancy

### ❌ Cons:

- More complex (coordination, state sharing)
- Requires load balancing, distributed caching, etc.

---

## 🔁 3. **Auto-Scaling**

### 🔹 What It Is:

- Automatically increase/decrease compute resources based on demand (CPU, memory, QPS).

### 📌 Example:

- AWS Auto Scaling, Kubernetes Horizontal Pod Autoscaler

### ✅ Pros:

- Cost-efficient
- Dynamic load handling

### ❌ Cons:

- Requires monitoring & threshold tuning
- Can cause delays during scale-up

---

## ⚖️ 4. **Load Balancing**

### 🔹 What It Is:

- Distribute incoming requests across multiple servers/nodes.

### 📌 Example:

- NGINX, HAProxy, AWS ALB/ELB, Azure Load Balancer

### ✅ Pros:

- Avoids bottlenecks
- Enables horizontal scaling

---

## 🧩 5. **Database Scaling**

### A. **Vertical DB Scaling** – bigger DB server

### B. **Read Replicas** – distribute read load

### C. **Sharding** – split DB into partitions by ID/hash/range

### 📌 Example:

- User IDs 1-10M on DB1, 10M–20M on DB2 (range sharding)

### ✅ Pros:

- Allows DB to scale with data

### ❌ Cons:

- Sharding adds **complexity** (joins, consistency)

---

## ☁️ 6. **Microservices & Service Decomposition**

### 🔹 What It Is:

- Break monolith into independently scalable microservices.

### 📌 Example:

- Separate `auth`, `search`, `orders`, and `payments` services

### ✅ Pros:

- Each service can scale independently
- Better fault isolation

### ❌ Cons:

- Increases complexity (deployment, network, coordination)

---

## 🧠 7. **Caching**

### 🔹 What It Is:

- Store frequently used data closer to the user or app.

### 📌 Example:

- Redis, Memcached, CDN (Cloudflare, Akamai)

### Types:

- Client-side caching (browser cache)
- Server-side caching (in-app memory)
- Reverse proxy caching (CDN)

### ✅ Pros:

- Reduces DB/API load
- Improves latency and scale

---

## 📨 8. **Asynchronous Processing (Message Queues)**

### 🔹 What It Is:

- Offload heavy tasks (email, billing, ML) to queues.

### 📌 Example:

- Kafka, RabbitMQ, SQS, Celery + Redis

### ✅ Pros:

- Non-blocking
- Smoothens load spikes

---

## 🛰️ 9. **Content Delivery Networks (CDNs)**

### 🔹 What It Is:

- Cache and serve static content (images, JS, CSS) from edge locations.

### 📌 Example:

- Cloudflare, Akamai, AWS CloudFront

### ✅ Pros:

- Reduces latency
- Offloads origin servers

---

## ⛓️ 10. **Data Partitioning / Sharding**

### 🔹 What It Is:

- Split data across multiple stores by key, region, etc.

### 📌 Example:

- Use user ID modulo to decide which database to write to

### ✅ Pros:

- Scales write-heavy systems
- Allows parallelism

---

## 🗃️ 11. **Concurrency & Threading**

### 🔹 What It Is:

- Use asynchronous/non-blocking I/O or multi-threading to handle more concurrent users.

### 📌 Example:

- Node.js (event loop), Go routines, Python async

### ✅ Pros:

- Improves server throughput
- More efficient resource usage

---

## 🧠 12. **CDN for API Acceleration**

### 🔹 What It Is:

- Cache GET/HEAD API responses at edge locations.

### 📌 Example:

- GraphQL/CDN combo with cache key variation

### ✅ Pros:

- Faster APIs
- Reduces server hits

---

## 🧭 Choosing the Right Scaling Strategy

| Use Case | Suggested Scaling |
| --- | --- |
| Handling more users | Horizontal scaling + Load Balancing |
| Spiky traffic | Auto-scaling + Asynchronous queues |
| Slower DB | Read Replicas + Caching |
| Latency issues | CDN + Geo-distribution |
| Monolith hitting limits | Break into Microservices |
| Too many writes | DB Sharding |

---

Would you like a **diagram** of how these work together (e.g., modern scalable web architecture), or examples in cloud services like **AWS**, **Azure**, or **Kubernetes**?

### Ways of making system highly available

Making a system **Highly Available (HA)** means ensuring it continues to operate with **minimal downtime**, even in the face of failures—like server crashes, network outages, or traffic spikes.

Here’s a comprehensive breakdown of the **key strategies and components** used to design **highly available systems**:

---

## 🧩 1. **Redundancy**

### ✅ What It Means:

- Have **duplicate resources** (servers, databases, services) so failure of one doesn’t affect the system.

### 🔧 Examples:

- Multiple app servers
- Master-slave or multi-master databases
- Dual power supplies or network cards

---

## ⚖️ 2. **Load Balancing**

### ✅ What It Means:

- Distribute traffic across **multiple servers or services** to avoid overloading any one of them.

### 🔧 Tools:

- **NGINX**, **HAProxy**, **AWS ALB/ELB**, **Azure Load Balancer**

### 💡 Features:

- Health checks
- Automatic rerouting if a node goes down
- Can be **global (Geo LB)** or **local**

---

## ☁️ 3. **Failover Mechanism**

### ✅ What It Means:

- **Automatic switch** to a standby system (or replica) when the primary fails.

### 🔧 Examples:

- **Database failover**: Primary to replica
- DNS failover: Traffic switches to a different region/data center

---

## 📦 4. **Replication (Database & Services)**

### ✅ What It Means:

- Keep **real-time or near-real-time copies** of databases and services.

### 🔧 Types:

- **Master-slave** (read replicas)
- **Multi-master** (active-active)
- **Log shipping** / **Change Data Capture (CDC)**

### 📌 Helps with:

- High availability
- Read scalability
- Backup and recovery

---

## 🌐 5. **Geo-Distribution / Multi-Region Deployment**

### ✅ What It Means:

- Deploy your app and database in **multiple geographical regions**.

### 🔧 Use Cases:

- Disaster recovery (data center down)
- Serve users from the closest region (reduced latency)
- Multi-region databases (e.g., **Cosmos DB**, **AWS Aurora Global DB**)

---

## 🚥 6. **Health Checks & Monitoring**

### ✅ What It Means:

- Continuously monitor the health of your services and trigger recovery actions if something goes down.

### 🔧 Tools:

- Prometheus + Grafana
- AWS CloudWatch
- Kubernetes liveness/readiness probes

---

## 🧪 7. **Graceful Degradation**

### ✅ What It Means:

- If part of the system fails, still provide **limited or partial functionality**.

### 🔧 Example:

- If payment gateway fails, allow users to browse but not checkout
- Show cached data when database is down

---

## 🔁 8. **Auto-Scaling**

### ✅ What It Means:

- Automatically increase or decrease the number of instances based on load.

### 🔧 Tools:

- AWS Auto Scaling Groups
- Kubernetes Horizontal Pod Autoscaler (HPA)

### 📌 Helps with:

- Traffic spikes
- Avoiding system overloads

---

## 🧱 9. **Stateless Services + Shared Storage**

### ✅ What It Means:

- Keep application servers **stateless** so any request can be handled by any instance.

### 🔧 Use:

- Sessions stored in Redis, not in memory
- Files stored in S3, not on local disk

### 📌 Enables:

- Easy horizontal scaling
- Easy failover

---

## 🧊 10. **Caching & CDN**

### ✅ What It Means:

- Reduce dependency on backend by caching results or static content.

### 🔧 Tools:

- Redis/Memcached (data caching)
- Cloudflare, Akamai, CloudFront (static/CDN caching)

### 📌 Helps with:

- Availability during backend failure
- Reduced load on DB or API

---

## 🔐 11. **Disaster Recovery (DR) Strategy**

### ✅ What It Means:

- Predefined process to restore service/data after **a catastrophic event**.

### 🔧 Types:

- **Active-active** (hot-hot): All regions always on
- **Active-passive** (hot-warm): One region standby
- **Backup-restore** (cold): Restore from backup

---

## ⛓️ 12. **Message Queues & Asynchronous Processing**

### ✅ What It Means:

- Handle spikes/failures by **queuing requests** instead of dropping them.

### 🔧 Tools:

- Kafka, RabbitMQ, SQS

### 📌 Use Cases:

- Email sending
- Order processing
- Retry mechanisms

---

## 🔐 13. **Reliable DNS with Health Checks**

### ✅ What It Means:

- DNS services with **routing policies and failover**.

### 🔧 Tools:

- AWS Route53, Google Cloud DNS
- Latency-based routing, failover routing

---

## 🗃️ 14. **Backups & Snapshots**

### ✅ What It Means:

- Regular backups of data and system state.

### 🔧 Use:

- Database dumps
- Filesystem snapshots (EBS Snapshots)

### 📌 Required for:

- Recovery in case of data corruption
- Ransomware protection

---

## ✅ Summary Table

| Strategy | Handles Failures | Prevents Downtime | Key Tool/Concept |
| --- | --- | --- | --- |
| Redundancy | ✅ | ✅ | Clustering |
| Load Balancing | ✅ | ✅ | NGINX, AWS ELB |
| Failover Systems | ✅ | ✅ | DNS Failover, DB Failover |
| Replication (DB/services) | ✅ | ✅ | Read replicas, CDC |
| Multi-region Deployment | ✅ | ✅ | AWS multi-region setup |
| Auto Scaling | ✅ | ✅ | Kubernetes HPA |
| Health Checks & Monitoring | ✅ | ⚠️ | Prometheus, ELK, Grafana |
| Graceful Degradation | ⚠️ | ✅ | Cached fallbacks |
| Stateless Apps + Shared Storage | ✅ | ✅ | Redis, S3 |
| CDN & Caching | ⚠️ | ✅ | Cloudflare, Redis |
| Async Queues | ✅ | ⚠️ | Kafka, SQS |
| Backups | ✅ | ❌ | Snapshots, cold DR |

---

