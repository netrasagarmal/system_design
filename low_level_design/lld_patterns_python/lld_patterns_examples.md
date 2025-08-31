## 🏭 1. **Factory Pattern**

> Creates objects without specifying exact class names.
> 

### Real-World Scenarios:

1. **Document Reader** – Based on file type (`.pdf`, `.docx`, `.txt`), return the appropriate parser.
2. **Notification System** – Create `Email`, `SMS`, or `Push` notification objects from a factory.
3. **Bank Account Opening** – Create `Savings`, `Current`, or `Salary` account types dynamically.
4. **Vehicle Rental App** – Choose `Bike`, `Car`, or `Truck` based on user request.
5. **Button Creation in GUI** – Generate OS-specific buttons (`WindowsButton`, `MacButton`).
6. **Payment Gateway** – Return appropriate gateway handler (`Stripe`, `Razorpay`, `PayPal`).
7. **Game Enemy Spawner** – Spawn `Orc`, `Goblin`, or `Dragon` objects based on level.
8. **Chart Creator** – Generate `BarChart`, `PieChart`, or `LineChart` based on data type.

---

## 🏢 2. **Abstract Factory Pattern**

> Creates families of related objects without specifying concrete classes.
> 

### Real-World Scenarios:

1. **Cross-platform UI Toolkit** – Create `Button`, `Checkbox`, `Menu` for `Windows`, `Mac`, or `Linux`.
2. **Theme System** – Produce `DarkButton`, `DarkTextbox`, etc., or `Light*` equivalents.
3. **Game Level Setup** – Each level has a different enemy, background, and soundtrack.
4. **Car Manufacturing Plant** – Produce `SUV`, `Sedan`, `Hatchback` models with different interiors & engines.
5. **Smart Home Kit** – Build compatible `Lights`, `Sensors`, `Cameras` for different ecosystems like `Apple Home`, `Google Home`, or `Alexa`.
6. **Cloud SDK Generator** – Create `StorageClient`, `ComputeClient`, `BillingClient` for AWS, Azure, or GCP.
7. **E-commerce UI Design** – Produce `MobileHeader`, `MobileFooter` vs `DesktopHeader`, `DesktopFooter`.
8. **Database Driver Factory** – Produce `MySQLDriver`, `PostgreSQLDriver`, `MongoDBDriver` with related query builders.

---

## 🏗️ 3. **Builder Pattern**

> Construct complex objects step by step.
> 

### Real-World Scenarios:

1. **Pizza Ordering System** – Choose crust, cheese, toppings, size in a step-by-step manner.
2. **Resume Builder Website** – User builds resume by adding sections like skills, projects, education, etc.
3. **Car Customizer** – Add sunroof, engine type, interior, color in configurable steps.
4. **Computer Builder** – Choose CPU, GPU, RAM, SSD, monitor for a custom PC.
5. **Hotel Booking Engine** – Add room, breakfast, airport pickup, early check-in step-by-step.
6. **Chatbot Response Builder** – Compose structured response with text, buttons, images.
7. **Email Composer** – Add subject, body, attachments, signature before sending.
8. **Report Generator** – Add title, table of contents, sections, charts before exporting as PDF.

---

## 🌳 4. **Composite Pattern**

> Treat individual and groups of objects uniformly.
> 

### Real-World Scenarios:

1. **File System** – Folders can contain files or other folders, but all respond to `.list()`.
2. **Company Org Chart** – Managers can contain employees or other managers.
3. **GUI Components** – Windows contain panels, which contain buttons and labels.
4. **Menu System** – A `MenuItem` or nested `SubMenu` can both be clicked or shown.
5. **Product Bundles** – A bundle can include individual items or other bundles.
6. **Project Task Breakdown** – A task can have subtasks and be treated the same.
7. **HTML/XML DOM Tree** – Elements can have nested children or be leaf nodes.
8. **Drawing App** – Shapes like circles or groups of shapes can be rendered the same way.

---

## 🔌 5. **Adapter Pattern**

> Bridge incompatible interfaces.
> 

### Real-World Scenarios:

1. **Phone Charger Plug** – Use adapter to fit foreign sockets.
2. **Legacy API to New API** – Convert old method names to new interfaces.
3. **Payment Integration** – Adapt `Stripe` or `Razorpay` APIs to your own app’s format.
4. **Third-Party Library Integration** – Wrap a third-party class to match your existing class design.
5. **USB to HDMI Adapter** – Connect old laptops to modern monitors.
6. **Database to ORM** – Adapt raw SQL results to model objects.
7. **Unit Converter App** – Adapt metric values to imperial and vice versa.
8. **Game Controller Adapter** – Use a PS controller on an Xbox using an adapter wrapper.

---

## 🧾 6. **Facade Pattern**

> Simplify interaction with complex systems.
> 

### Real-World Scenarios:

1. **Home Theater System** – Pressing “Watch Movie” turns on TV, speakers, streaming.
2. **Bank Account API** – Facade exposes simple methods like `transfer_funds()`, while hiding auth, balance check, etc.
3. **Hotel Booking Site** – Internally queries hotels, flights, and taxis, but exposes 1 search bar.
4. **E-commerce Checkout** – One `place_order()` method calls inventory, payment, and shipping systems.
5. **Computer Startup** – Power button starts CPU, memory, disk, and loads OS.
6. **Car Ignition** – One button internally starts fuel injection, motor, battery, etc.
7. **Travel Aggregator** – Aggregates from multiple APIs like MakeMyTrip, IRCTC, RedBus.
8. **CRM Dashboard** – User sees a single interface, but it pulls data from dozens of microservices.

---

## 🧠 7. **Strategy Pattern**

> Select algorithm behavior at runtime.
> 

### Real-World Scenarios:

1. **Sorting Strategy** – Choose bubble sort, merge sort, or quick sort based on data size.
2. **Authentication System** – Use `OAuth`, `JWT`, or `BasicAuth` as strategies.
3. **Travel App** – Choose `fastest`, `cheapest`, or `scenic` route algorithms.
4. **Compression Tool** – Choose `ZIP`, `RAR`, or `TAR` based on user’s preference.
5. **Tax Calculator** – Strategy varies for `Individual`, `Company`, or `Freelancer`.
6. **Search Engine** – Use different ranking algorithms (`PageRank`, `ML Rank`, `Time-based`).
7. **Image Filter App** – Choose from grayscale, sepia, contrast filters at runtime.
8. **Recommendation Engine** – Switch between `collaborative`, `content-based`, or `hybrid`.

---

## 👀 8. **Observer Pattern**

> Notify multiple objects when a subject changes.
> 

### Real-World Scenarios:

1. **Stock Price App** – Multiple UI widgets update when stock price changes.
2. **YouTube Channel** – Subscribers get notified when new video is uploaded.
3. **Weather App** – All displays (LED, Mobile, TV) get notified when weather changes.
4. **Chat App** – All open chat windows update when a message arrives.
5. **Auction System** – Notify bidders in real time when price changes.
6. **Build System** – On code push, trigger test suite, deployment, notifications.
7. **Smart Home** – Sensors notify devices (lights, alarm, doors) when motion is detected.
8. **File Watcher** – IDE plugins react when a source file is updated or saved.

---

## 1. Factory Pattern

1. **Database Connection Factory** - Creates different database connections (MySQL, PostgreSQL, Oracle) based on configuration without exposing connection details
2. **UI Component Factory** - Generates different UI elements (buttons, text fields, dropdowns) based on the operating system or theme
3. **Payment Gateway Factory** - Creates payment processors (PayPal, Stripe, Square) based on user selection or geographic location
4. **Document Parser Factory** - Produces parsers for different file formats (PDF, Word, Excel) based on file extension
5. **Logging Framework Factory** - Creates different loggers (file logger, console logger, database logger) based on environment settings
6. **Vehicle Manufacturing** - Produces different vehicle types (sedan, SUV, truck) in an automotive assembly line
7. **Game Character Factory** - Creates different character types (warrior, mage, archer) in RPG games based on player choice
8. **Email Service Factory** - Generates email service instances (SMTP, SendGrid, Amazon SES) based on configuration
9. **Image Format Converter Factory** - Creates converters for different image formats (JPEG, PNG, GIF) based on input/output requirements
10. **Notification Factory** - Produces different notification types (push, SMS, email) based on user preferences and device capabilities

## 2. Abstract Factory Pattern

1. **Cross-Platform GUI Toolkit** - Creates families of UI components (Windows/Mac/Linux styles) ensuring all components match the platform's look and feel
2. **Gaming Console Development** - Produces compatible hardware components (controllers, memory cards, cables) for specific console generations
3. **Furniture Manufacturing** - Creates coordinated furniture sets (modern/classic/rustic styles) where all pieces complement each other
4. **Database Driver Ecosystem** - Generates complete database access layers (connection, command, transaction objects) for different database vendors
5. **Mobile App Theming** - Creates consistent UI element families (dark theme, light theme, high contrast) across entire applications
6. **Automotive Parts Manufacturing** - Produces compatible part families (engine, transmission, electronics) for specific car models
7. **Restaurant Chain Branding** - Creates coordinated brand elements (menus, signage, uniforms, packaging) for different restaurant concepts under one company
8. **Software Development Tools** - Generates integrated development environments with matching components (editor, debugger, compiler) for different programming languages
9. **Medical Device Ecosystems** - Creates compatible medical equipment families (monitors, sensors, software) that work seamlessly together
10. **Smart Home Systems** - Produces coordinated smart device families (lights, thermostats, security) that integrate perfectly within specific ecosystems

## 3. Builder Pattern

1. **SQL Query Construction** - Builds complex database queries step-by-step with optional clauses (WHERE, JOIN, ORDER BY, GROUP BY)
2. **Email Composition** - Constructs emails by adding recipients, subject, body, attachments, formatting, and delivery options gradually
3. **Computer Configuration** - Assembles custom computers by selecting CPU, RAM, storage, graphics card, and peripherals piece by piece
4. **Resume/CV Builder** - Creates professional resumes by adding sections like experience, education, skills, and certifications incrementally
5. **Meal Planning** - Builds meal plans by selecting appetizers, main courses, sides, desserts, and dietary restrictions step by step
6. **Travel Itinerary Planning** - Constructs trip plans by adding destinations, accommodations, activities, transportation, and dates progressively
7. **Form Creation** - Builds web forms by adding various field types, validation rules, styling, and submission handlers systematically
8. **Report Generation** - Constructs business reports by adding headers, data sections, charts, filters, and formatting options incrementally
9. **House Construction Planning** - Builds house blueprints by adding rooms, fixtures, materials, and architectural features step by step
10. **Game Character Customization** - Creates game characters by selecting appearance, abilities, equipment, and backstory elements gradually

## 4. Composite Pattern

1. **File System Hierarchy** - Treats individual files and directories uniformly, allowing operations on both single files and entire folder structures
2. **Company Organizational Chart** - Manages employees and departments equally, where departments contain other departments or individual employees
3. **Graphics Drawing Applications** - Handles individual shapes and grouped shapes identically, allowing operations on single objects or complex compositions
4. **Menu Systems** - Treats menu items and submenus uniformly, enabling consistent operations across simple items and nested menu structures
5. **Document Structure** - Manages paragraphs, sections, and chapters uniformly in word processors, allowing formatting operations on any level
6. **UI Component Hierarchies** - Handles individual widgets and container panels similarly, enabling consistent behavior across simple and complex UI elements
7. **Mathematical Expressions** - Treats numbers and complex expressions equally, allowing evaluation operations on both simple values and nested formulas
8. **Military Command Structure** - Manages individual soldiers and units uniformly, enabling orders to be given to single personnel or entire divisions
9. **Geographic Information Systems** - Handles individual locations and region groupings similarly, allowing mapping operations on points or geographic areas
10. **Product Catalog Organization** - Treats individual products and product categories uniformly, enabling operations on single items or entire product lines

## 5. Adapter Pattern

1. **Legacy System Integration** - Allows modern applications to communicate with older systems that use different data formats or protocols
2. **Third-Party API Integration** - Enables applications to work with external services that have different interfaces than expected
3. **Media Player Compatibility** - Allows music players to handle different audio formats by adapting various codec interfaces to a common playback interface
4. **Payment Processing Integration** - Enables e-commerce platforms to work with multiple payment providers having different API structures
5. **Database Migration** - Allows applications to switch between different database systems without changing the core application code
6. **Social Media Integration** - Enables applications to post content across different social platforms with varying API requirements
7. **Cloud Storage Services** - Allows file management applications to work with different cloud providers (AWS, Google Cloud, Azure) through uniform interfaces
8. **Email Service Integration** - Enables applications to send emails through different providers (Gmail, Outlook, Yahoo) using consistent methods
9. **Translation Services** - Allows applications to use different translation APIs (Google Translate, Microsoft Translator) through a common interface
10. **Authentication System Integration** - Enables applications to work with various identity providers (OAuth, LDAP, SAML) using standardized authentication flows

## 6. Facade Pattern

1. **Online Banking Interface** - Provides a simple interface that hides complex interactions with account management, transaction processing, and security systems
2. **Smart Home Control Panel** - Offers easy controls that manage complex interactions between lighting, heating, security, and entertainment systems
3. **Travel Booking Websites** - Presents simple search and booking interfaces that coordinate with multiple airlines, hotels, and car rental systems
4. **E-commerce Checkout Process** - Provides streamlined checkout that manages complex inventory, payment, shipping, and tax calculation systems
5. **Operating System APIs** - Offers simple programming interfaces that hide complex hardware interactions and system resource management
6. **Compiler Frontend** - Provides simple compilation commands that coordinate complex lexical analysis, parsing, optimization, and code generation processes
7. **Medical Diagnostic Systems** - Presents simple test ordering interfaces that manage complex laboratory equipment, result processing, and reporting systems
8. **Automated Trading Platforms** - Offers simple trading interfaces that handle complex market data analysis, risk assessment, and order execution systems
9. **Content Management Systems** - Provides easy publishing interfaces that manage complex database operations, file handling, and security protocols
10. **Video Streaming Services** - Presents simple viewing interfaces that coordinate complex content delivery, user authentication, and recommendation systems

## 7. Strategy Pattern

1. **Navigation Applications** - Switches between different routing algorithms (fastest route, shortest distance, avoid tolls) based on user preferences
2. **Image Compression Tools** - Uses different compression strategies (JPEG, PNG, WebP) depending on image type and quality requirements
3. **E-commerce Pricing** - Applies different pricing strategies (regular pricing, promotional discounts, bulk pricing, membership rates) based on customer type
4. **Game AI Behavior** - Changes AI strategies (aggressive, defensive, balanced) in games based on difficulty level or game state
5. **Data Sorting Applications** - Employs different sorting algorithms (quicksort, mergesort, heapsort) based on data size and type
6. **Investment Portfolio Management** - Uses different investment strategies (conservative, moderate, aggressive) based on client risk tolerance
7. **Text Processing** - Applies different formatting strategies (markdown, HTML, plain text) based on output requirements
8. **Backup Systems** - Implements different backup strategies (full backup, incremental, differential) based on data importance and storage constraints
9. **Marketing Campaigns** - Executes different campaign strategies (email, social media, print advertising) based on target demographics
10. **Security Authentication** - Uses different validation strategies (password, biometric, two-factor authentication) based on security level requirements

## 8. Observer Pattern

1. **Social Media Notifications** - Followers automatically receive updates when users they follow post new content, comment, or share posts
2. **Stock Market Applications** - Investors receive real-time updates when stock prices change, market alerts trigger, or portfolio values fluctuate
3. **News Subscription Services** - Subscribers automatically receive notifications when new articles are published in their areas of interest
4. **Weather Monitoring Systems** - Multiple displays and applications update automatically when weather sensors detect changes in conditions
5. **Auction Websites** - Bidders receive immediate notifications when someone outbids them or when auction status changes
6. **Email Client Applications** - Users receive notifications across all devices when new emails arrive in their inbox
7. **Collaboration Tools** - Team members receive automatic updates when documents are edited, comments are added, or project status changes
8. **Gaming Leaderboards** - Players receive notifications when their rankings change or when they achieve new milestones
9. **Inventory Management Systems** - Multiple departments receive alerts when stock levels change, items go out of stock, or reorder points are reached
10. **Home Security Systems** - Multiple monitoring stations and mobile devices receive immediate alerts when sensors detect motion, door openings, or alarm triggers

---

We'll cover:

1. ✅ Core classes and responsibilities
2. ✅ Relationships and structure
3. ✅ Support for multiple product categories
4. ✅ Add to cart + view catalog use cases
5. ✅ Applied Design Patterns
6. ✅ Applied SOLID principles

---

## 🛒 1. **Requirements Recap**

The system should allow:

- Multiple product categories (e.g., Electronics, Books, Clothing)
- Storing and retrieving product details
- Viewing/searching/browsing products
- Adding products to a cart
- Supporting extensibility for new categories

---

## 📦 2. **Core Components (Classes)**

| Component | Responsibility |
| --- | --- |
| `Product` | Abstract class/interface for all products |
| `Book`, `Electronics`, `Clothing` | Concrete product subclasses |
| `Category` | Group of products (e.g., “Books” or “Electronics”) |
| `Catalog` | Holds all categories and search functionality |
| `Cart` | Holds selected products and quantities |
| `CartItem` | A line item in the cart |
| `User` | Contains user's cart and browsing history |
| `ProductFactory` | Dynamically creates product instances |
| `ProductSearchService` | Search/filter functionality for catalog |

---

## 🧱 3. **Class Relationships Overview**

- `Catalog` has multiple `Category` objects
- Each `Category` has multiple `Product`s
- `Product` is a **base/abstract** class
    - `Book`, `Clothing`, etc., inherit from `Product`
- `Cart` has many `CartItem`s
    - Each `CartItem` links to a `Product`
- `User` has one `Cart`
- `ProductFactory` is used to **create products dynamically**

---

## 🧠 4. **Design Patterns Used**

| Pattern | Where & Why |
| --- | --- |
| **Factory** | `ProductFactory` to create products by category/type |
| **Strategy (optional)** | `ProductSearchService` can allow switching between filter strategies (e.g., price, name, rating) |
| **Composite** | `Catalog` → `Category` → `Product` form a hierarchy |
| **Facade (optional)** | A `CatalogFacade` can simplify APIs for search, category listing, and filters |
| **Builder (optional)** | Can be used if product creation is complex (e.g., custom PC build) |

---

## 🧱 5. **SOLID Principles Applied**

| Principle | Application |
| --- | --- |
| **S: Single Responsibility** | Each class has one responsibility (e.g., `Cart` handles cart logic only) |
| **O: Open/Closed** | Can add new product types without modifying base `Product` class |
| **L: Liskov Substitution** | A `Book` or `Electronics` object can be used wherever `Product` is expected |
| **I: Interface Segregation** | If products have special behaviors (like `DigitalDownloadable`), we can split interfaces |
| **D: Dependency Inversion** | High-level services like `CatalogService` depend on `Product` interface, not implementation |

---

## 🧩 6. **Key Class Definitions and Roles (no code)**

### 📘 Product (Abstract Base Class)

- Attributes: `id`, `name`, `description`, `price`, `category`
- Methods: `get_details()`

### 📚 Book / 👕 Clothing / 💻 Electronics (Concrete Products)

- Inherit from `Product`
- Add category-specific attributes (e.g., `author` for Book, `size` for Clothing)

### 🗂️ Category

- `name`, `description`
- `List<Product>`

### 📚 Catalog

- Holds all categories
- Provides methods to search/filter products

### 🛒 Cart

- Has many `CartItem`s
- Can `add_product(product, quantity)` or `remove_product(product)`

### 🧾 CartItem

- Holds reference to a `Product` and quantity

### 👤 User

- Contains `Cart` and maybe `Wishlist`, `SearchHistory`

### 🏭 ProductFactory

- Creates specific product types based on input data

---

## 🔎 7. **Typical Use Case Flows**

### ▶ View Product Catalog:

1. User interacts with `Catalog`
2. `Catalog` lists `Category` → shows list of `Product`s in each
3. `ProductSearchService` helps search by keyword/price/etc.

### 🛒 Add Product to Cart:

1. User clicks "Add to Cart" on a product
2. `Cart.add_product(product, quantity)` is called
3. If already in cart, update quantity. Else, create new `CartItem`.

---

## 🧠 Extensibility Considerations

- **Add a new category**: Create a new subclass of `Product`, plug it into `ProductFactory`
- **Add filtering (e.g., by brand)**: Extend `ProductSearchService` using Strategy pattern
- **Add recommendation system**: Use `Observer` for user behavior tracking

---

## 📝 Summary

| Feature | Design |
| --- | --- |
| Product Variety | Abstract class + Factory pattern |
| Easy Product Creation | Factory (or Builder) |
| Search & Filter | Strategy or Facade |
| Category Hierarchy | Composite pattern |
| Cart Operations | Cart + CartItem classes |
| Extendable | New products via Open/Closed principle |
| Clean Structure | SOLID adherence |

Here’s a **simple Python implementation** of the **Product Catalog Design** system we discussed.

It includes support for:

- Different product categories (Books, Electronics)
- Viewing product catalog
- Adding products to a cart

> 🔧 Patterns used: Factory, Composite, SOLID principles
> 
> 
> 🧠 Keep in mind this is simplified for LLD clarity, and can be expanded with DB integration, APIs, or services later.
> 

---

## ✅ 1. Product Classes (Base + Concrete)

```python
from abc import ABC, abstractmethod

# Base Product
class Product(ABC):
    def __init__(self, product_id, name, price, description):
        self.product_id = product_id
        self.name = name
        self.price = price
        self.description = description

    @abstractmethod
    def get_details(self):
        pass

# Book Product
class Book(Product):
    def __init__(self, product_id, name, price, description, author):
        super().__init__(product_id, name, price, description)
        self.author = author

    def get_details(self):
        return f"[Book] {self.name} by {self.author} - ₹{self.price}"

# Electronics Product
class Electronics(Product):
    def __init__(self, product_id, name, price, description, brand):
        super().__init__(product_id, name, price, description)
        self.brand = brand

    def get_details(self):
        return f"[Electronics] {self.brand} {self.name} - ₹{self.price}"

```

---

## 🏭 2. Product Factory

```python
class ProductFactory:
    @staticmethod
    def create_product(product_type, *args, **kwargs):
        if product_type == "book":
            return Book(*args, **kwargs)
        elif product_type == "electronics":
            return Electronics(*args, **kwargs)
        else:
            raise ValueError("Unknown product type")

```

---

## 📂 3. Category and Catalog (Composite)

```python
class Category:
    def __init__(self, name):
        self.name = name
        self.products = []

    def add_product(self, product):
        self.products.append(product)

    def list_products(self):
        return [p.get_details() for p in self.products]

class Catalog:
    def __init__(self):
        self.categories = []

    def add_category(self, category):
        self.categories.append(category)

    def view_catalog(self):
        for category in self.categories:
            print(f"\n--- {category.name} ---")
            for product in category.products:
                print(product.get_details())

```

---

## 🛒 4. Cart & CartItem

```python
class CartItem:
    def __init__(self, product, quantity):
        self.product = product
        self.quantity = quantity

    def get_item_total(self):
        return self.product.price * self.quantity

class Cart:
    def __init__(self):
        self.items = []

    def add_product(self, product, quantity):
        for item in self.items:
            if item.product.product_id == product.product_id:
                item.quantity += quantity
                return
        self.items.append(CartItem(product, quantity))

    def view_cart(self):
        print("\n🛒 Cart Contents:")
        total = 0
        for item in self.items:
            line = f"{item.product.name} x{item.quantity} = ₹{item.get_item_total()}"
            print(line)
            total += item.get_item_total()
        print(f"Total: ₹{total}")

```

---

## 👤 5. User Class

```python
class User:
    def __init__(self, name):
        self.name = name
        self.cart = Cart()

```

---

## ✅ 6. Example Usage

```python
# Create catalog and categories
catalog = Catalog()
books = Category("Books")
electronics = Category("Electronics")

# Add to catalog
catalog.add_category(books)
catalog.add_category(electronics)

# Create products using factory
book1 = ProductFactory.create_product("book", 1, "Atomic Habits", 450, "Self-help guide", author="James Clear")
book2 = ProductFactory.create_product("book", 2, "Clean Code", 600, "Programming book", author="Robert Martin")

elec1 = ProductFactory.create_product("electronics", 3, "Smartphone", 15000, "Android 5G", brand="Samsung")
elec2 = ProductFactory.create_product("electronics", 4, "Laptop", 55000, "16GB RAM", brand="Dell")

# Add products to categories
books.add_product(book1)
books.add_product(book2)

electronics.add_product(elec1)
electronics.add_product(elec2)

# View catalog
catalog.view_catalog()

# User operations
user = User("Sagar")
user.cart.add_product(book1, 2)
user.cart.add_product(elec2, 1)
user.cart.view_cart()

```

---

## 🧠 Recap of Design Patterns & SOLID

### ✅ **Design Patterns**

- **Factory** → For product creation based on type
- **Composite** → Catalog → Categories → Products structure
- **(Optional)** Strategy → Search service could be added
- **(Optional)** Builder → For custom products or configurable setups

### ✅ **SOLID**

- **S**: Each class has single responsibility (Cart, Product, etc.)
- **O**: New product types can be added without modifying base code
- **L**: Subclasses (`Book`, `Electronics`) can replace base `Product`
- **I**: Can extend interfaces if needed (e.g., `DigitalProduct`)
- **D**: High-level modules (e.g., Catalog) use `Product` abstraction

---

Here’s a complete **Low-Level Design (LLD)** for a **User and Admin Login & Management System** with:

- ✅ Account creation and login
- ✅ Admin role and permissions
- ✅ Organization and team structure
- ✅ Roles/permissions for users
- ✅ Simple Python code implementation
- ✅ Applied **Design Patterns** and **SOLID principles**

---

## 🧾 1. System Requirements

### 🎯 Functional Requirements:

- Users can **create account**, **login**
- Admins can also **login** and:
    - Assign **roles** or **permissions** to users (e.g., VIEW_REPORTS, MANAGE_TEAM)
    - Organize users into **teams** under an **organization**
- Support role-based access control (RBAC)

### 📐 Non-Functional:

- Scalable and modular
- Extendable for more roles, teams, and permissions

---

## 🧱 2. Core Classes Overview

| Class | Responsibility |
| --- | --- |
| `User` | Represents a basic user with login and assigned roles |
| `Admin` | Inherits `User` and can manage roles, teams |
| `Role` | Represents a named role (e.g., "Manager") |
| `Permission` | Fine-grained permission like "VIEW_REPORTS" |
| `Team` | Group of users |
| `Organization` | Owns multiple teams |
| `AuthService` | Handles login and account creation |
| `RoleService` | Assigns/removes roles & permissions |
| `PermissionService` | Manages access checks |

---

## 🧠 3. Design Patterns Used

| Pattern | Use |
| --- | --- |
| **Factory** | Create `User` or `Admin` object dynamically |
| **Strategy (optional)** | Different authentication methods (e.g., email, OTP) |
| **Facade** | `AuthService` hides complexity of login & user creation |
| **Composite** | `Organization → Teams → Users` hierarchy |

---

## ✅ 4. Applied SOLID Principles

| Principle | How it's used |
| --- | --- |
| **S**ingle Responsibility | Auth logic, role logic, and permission logic are separated |
| **O**pen/Closed | Add new roles/permissions without modifying core classes |
| **L**iskov Substitution | `Admin` can be used anywhere `User` is used |
| **I**nterface Segregation | Permissions and roles kept modular |
| **D**ependency Inversion | Services depend on abstractions not concrete classes |

---

## ✅ 5. Python Code Implementation (Simplified)

### 📌 Entities: `User`, `Admin`, `Role`, `Permission`

```python
class Permission:
    def __init__(self, name):
        self.name = name

class Role:
    def __init__(self, name):
        self.name = name
        self.permissions = set()

    def add_permission(self, permission):
        self.permissions.add(permission)

    def has_permission(self, permission):
        return permission in self.permissions

class User:
    def __init__(self, username, password):
        self.username = username
        self.password = password
        self.roles = set()

    def add_role(self, role):
        self.roles.add(role)

    def has_permission(self, permission):
        return any(role.has_permission(permission) for role in self.roles)

class Admin(User):
    def __init__(self, username, password):
        super().__init__(username, password)

```

---

### 🏢 Org & Team Structure

```python
class Team:
    def __init__(self, name):
        self.name = name
        self.members = []

    def add_member(self, user):
        self.members.append(user)

class Organization:
    def __init__(self, name):
        self.name = name
        self.teams = []

    def add_team(self, team):
        self.teams.append(team)

```

---

### 🔐 AuthService (Login & Register)

```python
class AuthService:
    def __init__(self):
        self.users = {}

    def register_user(self, username, password, is_admin=False):
        if username in self.users:
            raise ValueError("User already exists")
        user = Admin(username, password) if is_admin else User(username, password)
        self.users[username] = user
        return user

    def login(self, username, password):
        user = self.users.get(username)
        if not user or user.password != password:
            raise ValueError("Invalid credentials")
        return user

```

---

### ⚙️ Role & Permission Services

```python
class RoleService:
    def __init__(self):
        self.roles = {}

    def create_role(self, role_name):
        role = Role(role_name)
        self.roles[role_name] = role
        return role

    def assign_role_to_user(self, user, role_name):
        if role_name in self.roles:
            user.add_role(self.roles[role_name])

class PermissionService:
    def create_permission(self, name):
        return Permission(name)

    def assign_permission_to_role(self, role, permission):
        role.add_permission(permission)

```

---

### ✅ 6. Example Usage

```python
# Initialize services
auth_service = AuthService()
role_service = RoleService()
perm_service = PermissionService()

# Create permissions
view_perm = perm_service.create_permission("VIEW_REPORTS")
manage_team_perm = perm_service.create_permission("MANAGE_TEAM")

# Create roles
manager_role = role_service.create_role("Manager")
admin_role = role_service.create_role("Admin")

# Assign permissions to roles
perm_service.assign_permission_to_role(manager_role, view_perm)
perm_service.assign_permission_to_role(admin_role, view_perm)
perm_service.assign_permission_to_role(admin_role, manage_team_perm)

# Register users
admin = auth_service.register_user("admin1", "adminpass", is_admin=True)
user1 = auth_service.register_user("user1", "userpass")

# Assign roles
role_service.assign_role_to_user(admin, "Admin")
role_service.assign_role_to_user(user1, "Manager")

# Create org structure
org = Organization("Acme Corp")
team = Team("Engineering")
org.add_team(team)
team.add_member(user1)

# Access check
print(user1.has_permission(view_perm))       # True
print(user1.has_permission(manage_team_perm))  # False

```

---

## 🧩 7. Recap

| Feature | Implementation |
| --- | --- |
| User/Admin | `User`, `Admin`, `AuthService` |
| Roles & Permissions | `Role`, `Permission`, assigned via `RoleService` |
| Org Structure | `Organization`, `Team`, `User` |
| Auth System | `AuthService` handles login and registration |
| Role Assignment | Admin assigns via service layer |
| Extensibility | Add more permissions/roles easily |

---
