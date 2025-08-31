## 🏭 1. Factory Pattern

### 🎯 Goal:

Create objects without exposing the exact class name to the client.

---

### 🍰 **Example 1: Cake Factory**

```python
class Cake:
    def eat(self):
        pass

class ChocolateCake(Cake):
    def eat(self):
        print("Eating Chocolate Cake")

class VanillaCake(Cake):
    def eat(self):
        print("Eating Vanilla Cake")

class CakeFactory:
    @staticmethod
    def get_cake(cake_type):
        if cake_type == "chocolate":
            return ChocolateCake()
        elif cake_type == "vanilla":
            return VanillaCake()
        else:
            raise ValueError("Unknown cake type")

# Client
cake = CakeFactory.get_cake("chocolate")
cake.eat()
```

---

### 🚗 **Example 2: Vehicle Factory**

```python
class Vehicle:
    def start(self):
        pass

class Car(Vehicle):
    def start(self):
        print("Car started")

class Bike(Vehicle):
    def start(self):
        print("Bike started")

class VehicleFactory:
    @staticmethod
    def create_vehicle(vtype):
        if vtype == "car":
            return Car()
        elif vtype == "bike":
            return Bike()
        else:
            raise ValueError("Unknown vehicle")

# Client
vehicle = VehicleFactory.create_vehicle("bike")
vehicle.start()
```

---

## 🏢 2. Abstract Factory Pattern

### 🎯 Goal:

Provide an interface to create families of related objects **without specifying their concrete classes**.

---

### 🛋️ **Example 1: Furniture Factory**

```python
# Abstract Products
class Chair:
    def sit(self): pass

class Table:
    def use(self): pass

# Concrete Products
class ModernChair(Chair):
    def sit(self):
        print("Sitting on modern chair")

class ModernTable(Table):
    def use(self):
        print("Using modern table")

class VictorianChair(Chair):
    def sit(self):
        print("Sitting on victorian chair")

class VictorianTable(Table):
    def use(self):
        print("Using victorian table")

# Abstract Factory
class FurnitureFactory:
    def create_chair(self): pass
    def create_table(self): pass

# Concrete Factories
class ModernFurnitureFactory(FurnitureFactory):
    def create_chair(self): return ModernChair()
    def create_table(self): return ModernTable()

class VictorianFurnitureFactory(FurnitureFactory):
    def create_chair(self): return VictorianChair()
    def create_table(self): return VictorianTable()

# Client
def client(factory: FurnitureFactory):
    chair = factory.create_chair()
    table = factory.create_table()
    chair.sit()
    table.use()

client(ModernFurnitureFactory())
```

---

### 🎮 **Example 2: Game UI Factory (Dark/Light Themes)**

```python
class Button:
    def render(self): pass

class TextBox:
    def render(self): pass

class DarkButton(Button):
    def render(self):
        print("Render dark button")

class DarkTextBox(TextBox):
    def render(self):
        print("Render dark textbox")

class LightButton(Button):
    def render(self):
        print("Render light button")

class LightTextBox(TextBox):
    def render(self):
        print("Render light textbox")

class UIFactory:
    def create_button(self): pass
    def create_textbox(self): pass

class DarkUIFactory(UIFactory):
    def create_button(self): return DarkButton()
    def create_textbox(self): return DarkTextBox()

class LightUIFactory(UIFactory):
    def create_button(self): return LightButton()
    def create_textbox(self): return LightTextBox()

# Client
def render_ui(factory: UIFactory):
    factory.create_button().render()
    factory.create_textbox().render()

render_ui(DarkUIFactory())
```

---

## 🏗️ 3. Builder Pattern

### 🎯 Goal:

Construct complex objects step by step.

---

### 🍔 **Example 1: Burger Builder**

```python
class Burger:
    def __init__(self):
        self.ingredients = []

    def show(self):
        print("Burger with:", ", ".join(self.ingredients))

class BurgerBuilder:
    def __init__(self):
        self.burger = Burger()

    def add_patty(self):
        self.burger.ingredients.append("patty")
        return self

    def add_cheese(self):
        self.burger.ingredients.append("cheese")
        return self

    def add_lettuce(self):
        self.burger.ingredients.append("lettuce")
        return self

    def build(self):
        return self.burger

# Client
burger = BurgerBuilder().add_patty().add_cheese().add_lettuce().build()
burger.show()
```

---

### 🧑‍🎓 **Example 2: Student Profile Builder**

```python
class Student:
    def __init__(self):
        self.name = None
        self.age = None
        self.course = None

    def show(self):
        print(f"Student: {self.name}, Age: {self.age}, Course: {self.course}")

class StudentBuilder:
    def __init__(self):
        self.student = Student()

    def set_name(self, name):
        self.student.name = name
        return self

    def set_age(self, age):
        self.student.age = age
        return self

    def set_course(self, course):
        self.student.course = course
        return self

    def build(self):
        return self.student

# Client
student = StudentBuilder().set_name("Alice").set_age(22).set_course("AI/ML").build()
student.show()
```

