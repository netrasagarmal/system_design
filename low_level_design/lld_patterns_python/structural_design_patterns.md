## 🌳 1. **Composite Pattern**

### 🎯 Goal:

Treat **individual objects and groups** of objects **uniformly**.

### Analogy:

A file system — folders contain files or other folders, but you can perform operations (like list or size) uniformly.

---

### 📁 **Example 1: File System (Folder + File)**

```python
class FileSystemComponent:
    def show(self):
        pass

class File(FileSystemComponent):
    def __init__(self, name):
        self.name = name

    def show(self):
        print(f"File: {self.name}")

class Folder(FileSystemComponent):
    def __init__(self, name):
        self.name = name
        self.children = []

    def add(self, component):
        self.children.append(component)

    def show(self):
        print(f"Folder: {self.name}")
        for child in self.children:
            child.show()

# Client
file1 = File("a.txt")
file2 = File("b.jpg")
folder1 = Folder("Documents")
folder1.add(file1)
folder1.add(file2)

root = Folder("Root")
root.add(folder1)
root.show()
```

---

### 👥 **Example 2: Employee Hierarchy**

```python
class Employee:
    def __init__(self, name, role):
        self.name = name
        self.role = role
        self.subordinates = []

    def add(self, emp):
        self.subordinates.append(emp)

    def show(self, level=0):
        print(" " * level * 2 + f"{self.name} - {self.role}")
        for sub in self.subordinates:
            sub.show(level + 1)

# Client
ceo = Employee("Alice", "CEO")
cto = Employee("Bob", "CTO")
dev1 = Employee("Carol", "Developer")
dev2 = Employee("Dave", "Developer")

cto.add(dev1)
cto.add(dev2)
ceo.add(cto)
ceo.show()
```

---

## 🔌 2. **Adapter Pattern**

### 🎯 Goal:

**Convert one interface to another** to make incompatible systems work together.

### Analogy:

A power adapter lets a UK plug work in an Indian socket.

---

### 🔌 **Example 1: Audio Player Adapter**

```python
class MediaPlayer:
    def play(self, filename):
        pass

class MP3Player(MediaPlayer):
    def play(self, filename):
        print(f"Playing MP3 file: {filename}")

class MP4Player:
    def play_mp4(self, filename):
        print(f"Playing MP4 file: {filename}")

class MP4Adapter(MediaPlayer):
    def __init__(self, mp4_player):
        self.mp4_player = mp4_player

    def play(self, filename):
        self.mp4_player.play_mp4(filename)

# Client
mp3 = MP3Player()
mp4 = MP4Adapter(MP4Player())

mp3.play("song.mp3")
mp4.play("video.mp4")
```

---

### ⚙️ **Example 2: US Plug to Indian Socket**

```python
class IndianSocket:
    def plug_in(self):
        print("Plugged into Indian socket")

class USPlug:
    def connect(self):
        print("Connected using US plug")

class Adapter(IndianSocket):
    def __init__(self, us_plug):
        self.us_plug = us_plug

    def plug_in(self):
        self.us_plug.connect()

# Client
us_plug = USPlug()
adapter = Adapter(us_plug)
adapter.plug_in()
```

---

## 📺 3. **Facade Pattern**

### 🎯 Goal:

Provide a **simple interface** to a complex subsystem.

### Analogy:

A TV remote hides all the internal complexity behind a few buttons.

---

### 📺 **Example 1: Home Theater Facade**

```python
class Projector:
    def on(self): print("Projector ON")
    def off(self): print("Projector OFF")

class SoundSystem:
    def on(self): print("Sound System ON")
    def off(self): print("Sound System OFF")

class StreamingApp:
    def play(self): print("Streaming started")
    def stop(self): print("Streaming stopped")

class HomeTheaterFacade:
    def __init__(self):
        self.projector = Projector()
        self.sound = SoundSystem()
        self.streaming = StreamingApp()

    def watch_movie(self):
        print("Starting movie...")
        self.projector.on()
        self.sound.on()
        self.streaming.play()

    def end_movie(self):
        print("Stopping movie...")
        self.streaming.stop()
        self.sound.off()
        self.projector.off()

# Client
ht = HomeTheaterFacade()
ht.watch_movie()
ht.end_movie()
```

---

### 🏦 **Example 2: Bank Facade**

```python
class Account:
    def verify(self): print("Account Verified")

class Loan:
    def check_history(self): print("Loan history checked")

class CreditScore:
    def check_score(self): print("Credit score checked")

class BankFacade:
    def __init__(self):
        self.account = Account()
        self.loan = Loan()
        self.score = CreditScore()

    def apply_for_loan(self):
        print("Loan Application Process:")
        self.account.verify()
        self.loan.check_history()
        self.score.check_score()

# Client
bank = BankFacade()
bank.apply_for_loan()
```

---

### ✅ Summary

| Pattern       | Real-Life Analogy            | Main Idea                              |
| ------------- | ---------------------------- | -------------------------------------- |
| **Composite** | Files & Folders, Org chart   | Treat part and whole objects the same  |
| **Adapter**   | Plug converter, Media player | Convert incompatible interfaces        |
| **Facade**    | TV remote, Bank manager      | Simple interface over a complex system |
