# SOLID Design Principles:

### 1. **S – Single Responsibility Principle (SRP)**

* **Definition:** A class/module should have **only one reason to change**.
* **Idea:** Each component should do **only one job**.
* **Why:** If a class has multiple responsibilities, changes in one area may unintentionally affect the other. Keeping it focused makes it easier to maintain and modify.
* **Example (theory):** If you mix "data storage" and "report generation" in the same class, you will have to change it both when storage rules change and when reporting needs change. Instead, separate them.

---

### 2. **O – Open/Closed Principle (OCP)**

* **Definition:** Software entities should be **open for extension but closed for modification**.
* **Idea:** You should be able to add new functionality **without modifying existing code**.
* **Why:** Prevents breaking stable, tested code when new requirements come.
* **Example (theory):** Instead of changing a class every time a new business rule is added, you design it so that new rules can be added through extensions (like plugging in new strategies).

---

### 3. **L – Liskov Substitution Principle (LSP)**

* **Definition:** Objects of a superclass should be **replaceable with objects of its subclasses** without breaking the system.
* **Idea:** Subclasses should behave consistently with what is expected from their parent class.
* **Why:** Ensures correct polymorphic behavior. If a subclass violates the expectations (e.g., removes functionality, changes meaning), it breaks the principle.
* **Example (theory):** If "Shape" has a method "calculateArea", every subclass (Circle, Rectangle, etc.) must provide a logically correct implementation, not something unexpected.

---

### 4. **I – Interface Segregation Principle (ISP)**

* **Definition:** Clients should not be forced to depend on interfaces they **do not use**.
* **Idea:** Prefer many small, specific interfaces over one large, general interface.
* **Why:** Avoids "fat" interfaces that make classes implement unnecessary methods.
* **Example (theory):** Instead of one "Giant Machine Interface" that requires methods for printing, scanning, faxing, and copying, you create smaller interfaces—"Printable," "Scannable," etc. A simple printer doesn’t need to implement faxing.

---

### 5. **D – Dependency Inversion Principle (DIP)**

* **Definition:** Depend on **abstractions**, not on concrete implementations.
* **Idea:** High-level modules should not depend on low-level modules; both should depend on abstractions.
* **Why:** Makes the system flexible, easier to change, and testable (you can swap implementations without touching the core logic).
* **Example (theory):** Instead of a system directly depending on a "MySQL Database," it depends on a "Database Interface." Tomorrow, you can switch to PostgreSQL or an in-memory DB without breaking high-level business logic.

---

### ✨ Summary in One Line Each

* **S (SRP):** One class = One responsibility.
* **O (OCP):** Add new features without changing old code.
* **L (LSP):** Subclasses must be true substitutes for their parents.
* **I (ISP):** Don’t force classes to implement what they don’t need.
* **D (DIP):** Depend on abstractions, not details.

---

### ✅ **Benefits of SOLID Design Principles**

1. **Improved Code Maintainability**

   * Each class or function has a single, well-defined responsibility.
   * Easier to understand, debug, and update code without accidentally breaking unrelated functionality.

2. **Better Code Reusability**

   * Well-separated classes and interfaces can be reused across different projects or modules.
   * Reduces duplication of logic.

3. **Scalability and Flexibility**

   * New features can be added without modifying existing stable code.
   * Example: Open/Closed Principle allows you to extend behavior without rewriting core logic.

4. **Easier Testing and Debugging**

   * Small, independent classes/modules are easier to unit test.
   * Mocking dependencies is simpler with interfaces and dependency inversion.

5. **Reduced Risk of Code Rot / Spaghetti Code**

   * Prevents tightly coupled designs that are hard to change later.
   * Encourages loosely coupled architecture, where components depend on abstractions, not implementations.

6. **Team Collaboration Friendly**

   * Developers can work on separate modules without interfering with each other’s code.
   * Clean interfaces and boundaries reduce merge conflicts and miscommunication.

7. **Faster Adaptation to Business Changes**

   * Since classes follow single responsibility and depend on abstractions, business logic can evolve without requiring a full system rewrite.

8. **Professional & Industry-Standard Practice**

   * Many enterprise-level systems follow SOLID, so it improves career growth and helps you align with professional software engineering standards.

---


## 🔹 1. **S – Single Responsibility Principle (SRP)**

> *A class should have only one reason to change.*

---

### ❌ **Violation Scenario 1: UserManager doing too much**

```python
class UserManager:
    def save_user(self): ...
    def validate_user(self): ...
    def send_email(self): ...
```

* **Violation:** Manages user logic, validation, and email — 3 responsibilities.

### ✅ **After SRP: Separate responsibilities**

```python
class UserStorage:
    def save_user(self): ...

class UserValidator:
    def validate_user(self): ...

class EmailService:
    def send_email(self): ...
```

---

### ❌ Scenario 2: `Invoice` handles business and DB logic

```python
class Invoice:
    def calculate_total(self): ...
    def save_to_db(self): ...
```

### ✅ Fix:

```python
class Invoice:
    def calculate_total(self): ...

class InvoiceRepository:
    def save(self, invoice): ...
```

---

### ❌ Scenario 3: `Report` generates and prints the report

```python
class Report:
    def generate_data(self): ...
    def print_report(self): ...
```

### ✅ Fix:

```python
class Report:
    def generate_data(self): ...

class ReportPrinter:
    def print(self, report): ...
```

---

## 🔹 2. **O – Open/Closed Principle (OCP)**

> *Software entities should be open for extension, but closed for modification.*

---

### ❌ Scenario 1: `DiscountCalculator` with `if` chains

```python
class DiscountCalculator:
    def calculate(self, user_type):
        if user_type == 'regular':
            return 10
        elif user_type == 'premium':
            return 20
```

### ✅ Fix using polymorphism:

```python
class Discount:
    def calculate(self): pass

class RegularDiscount(Discount):
    def calculate(self): return 10

class PremiumDiscount(Discount):
    def calculate(self): return 20
```

---

### ❌ Scenario 2: `ShapeAreaCalculator` using `type` checks

```python
class AreaCalculator:
    def calculate(self, shape):
        if isinstance(shape, Circle):
            return 3.14 * shape.r ** 2
        elif isinstance(shape, Square):
            return shape.side ** 2
```

### ✅ Fix:

```python
class Shape:
    def area(self): pass

class Circle(Shape):
    def area(self): return 3.14 * self.r ** 2
```

---

### ❌ Scenario 3: Payment system checking method type

```python
class Payment:
    def pay(self, method):
        if method == 'paypal': ...
        elif method == 'credit_card': ...
```

### ✅ Fix:

```python
class PaymentMethod:
    def pay(self): pass

class PayPal(PaymentMethod):
    def pay(self): ...
```

---

## 🔹 3. **L – Liskov Substitution Principle (LSP)**

> *Subtypes must be substitutable for their base types.*

---

### ❌ Scenario 1: Square inherits Rectangle but breaks behavior

```python
class Rectangle:
    def set_width(self, w): ...
    def set_height(self, h): ...

class Square(Rectangle):
    def set_width(self, w):
        self.width = self.height = w
```

* **Violation:** `Square` overrides in a way that breaks `Rectangle` expectations.

### ✅ Fix:

* Avoid inheritance; prefer composition or separate classes.

---

### ❌ Scenario 2: `Bird` → `Penguin` can't fly

```python
class Bird:
    def fly(self): ...

class Penguin(Bird):
    def fly(self): raise NotImplementedError()
```

### ✅ Fix:

* Split into `FlyingBird` and `NonFlyingBird`

```python
class Bird: pass
class FlyingBird(Bird):
    def fly(self): ...
```

---

### ❌ Scenario 3: `Animal` with `make_sound()` and `fly()`

```python
class Animal:
    def make_sound(self): ...
    def fly(self): ...

class Dog(Animal):
    def fly(self): raise Exception("I can't fly")
```

### ✅ Fix:

* Split responsibilities or use interfaces per capability.

---

## 🔹 4. **I – Interface Segregation Principle (ISP)**

> *Clients should not be forced to depend on interfaces they do not use.*

---

### ❌ Scenario 1: One big interface

```python
class Machine:
    def print(self): ...
    def scan(self): ...
    def fax(self): ...
```

* A `Printer` has to implement `scan()` and `fax()` unnecessarily.

### ✅ Fix:

```python
class Printer:
    def print(self): ...

class Scanner:
    def scan(self): ...
```

---

### ❌ Scenario 2: `Shape` interface forces all to implement all

```python
class Shape:
    def draw(self): ...
    def resize(self): ...
    def animate(self): ...
```

* `StaticShape` may not need `animate`.

### ✅ Fix:

Split into smaller interfaces.

---

### ❌ Scenario 3: `Vehicle` interface with `fly()`, `drive()`, `sail()`

* Forces a `Car` to implement `fly()` or `sail()`.

### ✅ Fix:

```python
class Drivable: def drive(self): ...
class Flyable: def fly(self): ...
```

---

## 🔹 5. **D – Dependency Inversion Principle (DIP)**

> *Depend on abstractions, not on concrete classes.*

---

### ❌ Scenario 1: High-level module depends on low-level

```python
class MySQLDatabase:
    def save(self): ...

class UserService:
    def save_user(self):
        db = MySQLDatabase()
        db.save()
```

### ✅ Fix:

```python
class Database(ABC):
    def save(self): pass

class MySQLDatabase(Database): ...

class UserService:
    def __init__(self, db: Database):
        self.db = db
```

---

### ❌ Scenario 2: Logger tightly coupled

```python
class FileLogger:
    def log(self, msg): ...

class App:
    def __init__(self):
        self.logger = FileLogger()
```

### ✅ Fix:

```python
class Logger(ABC):
    def log(self, msg): pass

class FileLogger(Logger): ...

class App:
    def __init__(self, logger: Logger):
        self.logger = logger
```

---

### ❌ Scenario 3: Email sender tightly bound

```python
class EmailSender:
    def send(self): ...

class NotificationService:
    def __init__(self):
        self.sender = EmailSender()
```

### ✅ Fix:

Use a `MessageSender` interface.
