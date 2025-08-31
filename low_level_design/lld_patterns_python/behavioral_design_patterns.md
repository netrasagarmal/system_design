## 🧠 1. **Strategy Pattern**

### 🎯 Goal:

Define a family of algorithms, encapsulate each one, and **make them interchangeable** at runtime.

---

### 💳 **Example 1: Payment Strategy**

```python
class PaymentStrategy:
    def pay(self, amount): pass

class CardPayment(PaymentStrategy):
    def pay(self, amount):
        print(f"Paid ₹{amount} using Card")

class UpiPayment(PaymentStrategy):
    def pay(self, amount):
        print(f"Paid ₹{amount} using UPI")

class Checkout:
    def __init__(self, strategy: PaymentStrategy):
        self.strategy = strategy

    def set_strategy(self, strategy: PaymentStrategy):
        self.strategy = strategy

    def pay_amount(self, amount):
        self.strategy.pay(amount)

# Client
checkout = Checkout(CardPayment())
checkout.pay_amount(100)

checkout.set_strategy(UpiPayment())
checkout.pay_amount(200)
```

---

### 🚗 **Example 2: Driving Strategy (Auto vs Manual)**

```python
class DrivingMode:
    def drive(self):
        pass

class ManualDriving(DrivingMode):
    def drive(self):
        print("Driving in Manual Mode")

class AutoDriving(DrivingMode):
    def drive(self):
        print("Driving in Auto Mode")

class Car:
    def __init__(self, mode: DrivingMode):
        self.mode = mode

    def set_mode(self, mode: DrivingMode):
        self.mode = mode

    def drive(self):
        self.mode.drive()

# Client
car = Car(ManualDriving())
car.drive()

car.set_mode(AutoDriving())
car.drive()
```

---

## 👀 2. **Observer Pattern**

### 🎯 Goal:

When one object changes state, **all its dependents are notified automatically**.

---

### 📺 **Example 1: YouTube Channel Subscribers**

```python
class Subscriber:
    def notify(self, message): pass

class User(Subscriber):
    def __init__(self, name):
        self.name = name

    def notify(self, message):
        print(f"{self.name} received notification: {message}")

class YouTubeChannel:
    def __init__(self):
        self.subscribers = []

    def subscribe(self, user: Subscriber):
        self.subscribers.append(user)

    def unsubscribe(self, user: Subscriber):
        self.subscribers.remove(user)

    def upload_video(self, title):
        print(f"Channel: Uploaded '{title}'")
        for user in self.subscribers:
            user.notify(f"New video: {title}")

# Client
channel = YouTubeChannel()
u1 = User("Alice")
u2 = User("Bob")

channel.subscribe(u1)
channel.subscribe(u2)

channel.upload_video("Strategy Pattern Explained")
```

---

### 🌡️ **Example 2: Weather Station**

```python
class Observer:
    def update(self, temperature):
        pass

class PhoneDisplay(Observer):
    def update(self, temperature):
        print(f"Phone Display: Temperature is {temperature}°C")

class LEDDisplay(Observer):
    def update(self, temperature):
        print(f"LED Display: Temperature is {temperature}°C")

class WeatherStation:
    def __init__(self):
        self.observers = []
        self.temperature = 0

    def add_observer(self, obs: Observer):
        self.observers.append(obs)

    def remove_observer(self, obs: Observer):
        self.observers.remove(obs)

    def set_temperature(self, temp):
        print(f"\nWeatherStation: Temperature changed to {temp}°C")
        self.temperature = temp
        self.notify_all()

    def notify_all(self):
        for obs in self.observers:
            obs.update(self.temperature)

# Client
station = WeatherStation()
phone = PhoneDisplay()
led = LEDDisplay()

station.add_observer(phone)
station.add_observer(led)

station.set_temperature(25)
station.set_temperature(30)
```

---

### ✅ Summary

| Pattern  | Real-Life Analogy             | What it Does                         |
| -------- | ----------------------------- | ------------------------------------ |
| Strategy | Choosing UPI/Card at checkout | Switch algorithm behavior at runtime |
| Observer | YouTube/Weather Notifications | One-to-many event notification       |

