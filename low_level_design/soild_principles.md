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
