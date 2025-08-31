# Low Level Design Patterns

Concept of **Creational**, **Structural**, and **Behavioral** design patterns with **real-world analogies**.

---

## 🧱 1. **Creational Patterns** – *"How objects are created"*
> Focus on how objects are created, while hiding the instantiation logic and making the system independent of how its objects are created, composed, or represented.
> 

### 🔑 Purpose:

- Handle object creation problems.
- Promote flexibility and reuse.

### 🎯 Theme: Object Creation

> Think of situations where **you don’t create something directly**, but rely on a structured way to get the object.

### ✅ Key Subtypes:

| Pattern | Purpose |
| --- | --- |
| **Singleton** | Ensures a class has **only one instance** and provides a global access point to it. |
| **Factory Method** | Lets subclasses decide which object to create — uses **inheritance**. |
| **Abstract Factory** | Creates families of related objects **without specifying concrete classes**. |
| **Builder** | Separates the construction of a complex object from its representation. |
| **Prototype** | Creates objects by **cloning** an existing instance rather than creating a new one. |
---

### 🍰 **Example 1: Bakery (Factory Pattern)**

* You don’t bake the cake yourself.
* You tell the bakery: “I want a chocolate cake.”
* The bakery (factory) makes the correct type and gives it to you.

**➡️ Code analogy:** You call `CakeFactory.createCake("chocolate")`, and it returns a `ChocolateCake` object.

---

### 🧍 **Example 2: President (Singleton Pattern)**

* There’s only **one president** of a country.
* You don’t "create" a new president; everyone refers to the **same person**.

**➡️ Code analogy:** `President.getInstance()` always returns the same object.

---

### 🍔 **Example 3: Burger Order (Builder Pattern)**

* You want a custom burger: add lettuce, remove onions, extra cheese.
* The builder lets you specify each step and creates the final burger.

**➡️ Code analogy:**

```python
BurgerBuilder().addLettuce().addCheese().removeOnion().build()
```

---

## 🏗️ 2. **Structural Patterns** – *"How objects are arranged and connected"*

> Deal with the composition of classes and objects — i.e., how to form larger structures while keeping them flexible and efficient.
> 

### 🔑 Purpose:

- Helps ensure that parts of a system **work together effectively**.
- Improve **code maintainability**, **scalability**, and **flexibility**.

### 🎯 Theme: Object Composition

> Think of these like **engineering blueprints** for connecting parts to build a bigger system.

### ✅ Key Subtypes:

| Pattern | Purpose |
| --- | --- |
| **Adapter** | Converts the interface of one class into another expected by the client. |
| **Decorator** | Adds new functionality to objects **dynamically** without modifying their structure. |
| **Facade** | Provides a simplified **interface** to a complex subsystem. |
| **Proxy** | Acts as a placeholder or controller for accessing the real object. |
| **Composite** | Treats individual objects and groups of objects **uniformly** (tree structures). |
| **Bridge** | Decouples abstraction from implementation so that both can vary independently. |
| **Flyweight** | Reduces memory usage by **sharing common parts** of objects instead of creating new ones repeatedly. |


---

### 🧩 **Example 1: Lego Blocks (Composite Pattern)**

* Each block can be small or a large collection of smaller blocks.
* Together, they form a complex structure like a house or robot.

**➡️ Code analogy:** A `Shape` interface has both `Circle` and `GroupOfShapes`, and both can be drawn.

---

### 🔌 **Example 2: Power Plug Adapter (Adapter Pattern)**

* You have a UK plug, but the socket is US-style.
* You use an adapter so that incompatible systems can work together.

**➡️ Code analogy:** `UKPlugAdapter` converts UK plug interface to US socket interface.

---

### 📺 **Example 3: TV Remote (Facade Pattern)**

* Remote gives **simple buttons** (power, volume) to control a **complex TV system** inside.
* You don’t deal with the wiring or signal processing.

**➡️ Code analogy:** `TVFacade.turnOn()` internally handles amplifier, signal input, screen, etc.

---

## 🤝 3. **Behavioral Patterns** – *"How objects communicate"*

> Concerned with object communication — how they interact, communicate, and delegate responsibilities.
> 

### 🔑 Purpose:

- Improve **communication between objects**.
- Manage complex control flows or responsibilities.

### 🎯 Theme: Object Collaboration & Interaction

> These are about **who does what**, **who talks to whom**, and **how**.

### ✅ Key Subtypes:

| Pattern | Purpose |
| --- | --- |
| **Strategy** | Selects an algorithm's behavior **at runtime** via composition. |
| **Observer** | Defines a **1-to-many dependency** where objects are notified of changes. |
| **Command** | Encapsulates a request as an object, allowing undo/redo, queuing. |
| **State** | Allows an object to **alter its behavior** when its internal state changes. |
| **Chain of Responsibility** | Passes a request along a chain of handlers until one handles it. |
| **Mediator** | Centralizes complex communication between multiple objects. |
| **Template Method** | Defines the skeleton of an algorithm in the superclass, letting subclasses fill in steps. |
| **Iterator** | Provides a way to access elements of a collection without exposing its structure. |
| **Visitor** | Allows new operations to be performed on object structures **without modifying them**. |
| **Interpreter** | Defines a grammar and interpreter for a language. |


---

### 📣 **Example 1: YouTube Channel (Observer Pattern)**

* You subscribe to a channel.
* Every time a new video is uploaded, you (and all subscribers) are notified.

**➡️ Code analogy:**

```python
channel.subscribe(user1)
channel.notifySubscribers("New Video!")
```

---

### 💳 **Example 2: Payment Options (Strategy Pattern)**

* At checkout, you choose UPI, Card, or Wallet.
* The app **dynamically** selects the algorithm based on your choice.

**➡️ Code analogy:**

```python
payment = CardPayment()
checkout.setPaymentStrategy(payment)
checkout.pay()
```

---

### 🧾 **Example 3: Restaurant Waiter Orders (Command Pattern)**

* You tell the waiter: "Bring me a Veg Burger."
* The waiter doesn’t make the burger, but passes the **command** to the chef.

**➡️ Code analogy:**

```python
Command order = new VegBurgerCommand(kitchen)
waiter.takeOrder(order)
```

---

## 🧠 Summary Table of Analogies

| Category       | Pattern   | Real-Life Analogy               | Meaning                                |
| -------------- | --------- | ------------------------------- | -------------------------------------- |
| **Creational** | Factory   | Bakery                          | You order a cake, don’t bake it        |
|                | Singleton | President                       | Only one instance exists               |
|                | Builder   | Custom Burger                   | Build step-by-step, your way           |
| **Structural** | Composite | Lego Blocks                     | Combine small objects to big one       |
|                | Adapter   | Power Plug Adapter              | Make incompatible things work together |
|                | Facade    | TV Remote                       | Simple interface over complex system   |
| **Behavioral** | Observer  | YouTube Subscriber Notification | Notify multiple people when one event  |
|                | Strategy  | Payment Options                 | Choose algorithm at runtime            |
|                | Command   | Restaurant Waiter               | Encapsulate request as object          |

