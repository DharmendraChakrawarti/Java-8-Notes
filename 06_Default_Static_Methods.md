# 6. Default & Static Methods in Interfaces

> Before Java 8, interfaces could only have **abstract methods** (no body). Java 8 changed this by allowing **default** and **static** methods inside interfaces.

---

## 🧠 Theory – Understanding Default & Static Methods

### What Changed in Java 8?
Before Java 8, an interface was like a **blank form** – it told you what fields to fill in, but gave you no help. Now, interfaces can include **pre-filled answers** (default methods) and **utility tools** (static methods).

### Real-Life Analogy – Software Update 📱
Imagine **WhatsApp** releases a new feature: "Message Reactions" 👍

- **Old way (before Java 8):** Every single phone user must manually download and install a patch for reactions to work. If even one user doesn't update → feature is broken for them.

- **New way (default methods):** WhatsApp adds the feature with a **default behavior** built in. Everyone gets it automatically. If you want to customize it, you can. If not, the default works fine.

**That's exactly what default methods do!** → They add new functionality to an interface without breaking existing classes.

### Real-Life Analogy – USB Ports 🔌
Think of a **USB standard** as an interface:
- Original USB 2.0 = interface with basic methods (transfer data)
- USB 3.0 adds a new feature: fast charging

Without default methods:
> *"Every device that uses USB must now be redesigned to support fast charging."* → Millions of devices break! 💥

With default methods:
> *"Fast charging is available by default. Old devices still work. New devices can override it for even faster charging."* → Nothing breaks! ✅

### Default vs Static Methods

| Feature | Default Method | Static Method |
|---------|---------------|---------------|
| **Real-life analogy** | A **pre-written template** in a form – use it or write your own | A **calculator on the wall** – anyone can use it, but you can't take it home |
| **Who can use it?** | Objects of implementing classes | Called on the interface directly |
| **Can be overridden?** | ✅ Yes | ❌ No |
| **Inherited by child?** | ✅ Yes | ❌ No |
| **Use case** | Add new behavior to existing interfaces | Utility/helper methods |

### The Diamond Problem 💎
When a class implements **two interfaces** that both have the same default method, Java doesn't know which one to use. This is called the **Diamond Problem** (because the inheritance diagram looks like a diamond ♦).

**Solution**: The class MUST override the method and explicitly choose which version to use.

> Think of it like this: Your **mom** says "Dinner at 7 PM" and your **dad** says "Dinner at 8 PM." You're confused → so **you decide** the time yourself!

---

## 6.1 The Problem Before Java 8

```java
// Old interface
interface Vehicle {
    void start();
    void stop();
}

// If we add a new method like fuel()...
interface Vehicle {
    void start();
    void stop();
    void fuel();  // 💥 ALL implementing classes MUST add this method now!
}
```

> Adding a new method to an interface **broke all existing classes** that implemented it. Default methods solve this problem.

---

## 6.2 Default Methods

A **default method** has a body (implementation) inside the interface. Classes that implement the interface **get it for free** – they don't have to override it.

```java
interface Vehicle {
    void start();
    void stop();

    // Default method – has a body
    default void honk() {
        System.out.println("Beep beep! 🚗");
    }
}

class Car implements Vehicle {
    @Override
    public void start() { System.out.println("Car started"); }

    @Override
    public void stop() { System.out.println("Car stopped"); }

    // honk() is inherited automatically – no need to override
}

// Usage
Car car = new Car();
car.start();  // Car started
car.honk();   // Beep beep! 🚗
car.stop();   // Car stopped
```

---

## 6.3 Overriding Default Methods

You **can** override a default method if you want different behavior.

```java
class Truck implements Vehicle {
    @Override
    public void start() { System.out.println("Truck started"); }

    @Override
    public void stop() { System.out.println("Truck stopped"); }

    @Override
    public void honk() {
        System.out.println("HOOOONK! 🚛");  // custom behavior
    }
}

Truck truck = new Truck();
truck.honk(); // HOOOONK! 🚛
```

---

## 6.4 Diamond Problem – Multiple Interfaces with Same Default Method

What happens when a class implements **two interfaces** that have the **same default method**?

```java
interface InterfaceA {
    default void greet() {
        System.out.println("Hello from A");
    }
}

interface InterfaceB {
    default void greet() {
        System.out.println("Hello from B");
    }
}

// ❌ This will NOT compile – ambiguity!
// class MyClass implements InterfaceA, InterfaceB { }

// ✅ Fix: Override the method and choose which one to use
class MyClass implements InterfaceA, InterfaceB {
    @Override
    public void greet() {
        // Option 1: Choose one
        InterfaceA.super.greet();

        // Option 2: Write your own
        // System.out.println("Hello from MyClass");
    }
}

MyClass obj = new MyClass();
obj.greet(); // Hello from A
```

### Resolution Rules
| Scenario | Rule |
|----------|------|
| Class vs Interface | **Class always wins** |
| Sub-interface vs Super-interface | **More specific interface wins** |
| Two unrelated interfaces | **Must override** – compiler error otherwise |

---

## 6.5 Static Methods in Interfaces

Static methods belong to the **interface itself**, not to objects.

```java
interface MathUtils {

    // Static method – called on the interface directly
    static int add(int a, int b) {
        return a + b;
    }

    static int multiply(int a, int b) {
        return a * b;
    }
}

// Usage – call on the interface name
int sum = MathUtils.add(5, 3);         // 8
int product = MathUtils.multiply(5, 3); // 15

// ❌ Cannot call on an implementing class
// MyClass.add(5, 3); // COMPILE ERROR
```

### Real-world Example – Factory Pattern
```java
interface Logger {
    void log(String message);

    // Factory method
    static Logger consoleLogger() {
        return message -> System.out.println("[LOG] " + message);
    }

    static Logger fileLogger(String filename) {
        return message -> {
            // write to file (simplified)
            System.out.println("[FILE:" + filename + "] " + message);
        };
    }
}

// Usage
Logger console = Logger.consoleLogger();
Logger file = Logger.fileLogger("app.log");

console.log("Hello");    // [LOG] Hello
file.log("Error!");       // [FILE:app.log] Error!
```

---

## 6.6 Real-world Example – Evolving an Interface

```java
// Version 1 – original interface
interface Sortable {
    void sort();
}

// Version 2 – added default method (no breaking change!)
interface Sortable {
    void sort();

    default void sortDescending() {
        sort();
        System.out.println("Then reversing...");
    }

    static Sortable natural() {
        return () -> System.out.println("Natural sort");
    }
}

// Old class still works without changes
class MyList implements Sortable {
    @Override
    public void sort() {
        System.out.println("Sorting my list...");
    }
}

MyList list = new MyList();
list.sort();            // Sorting my list...
list.sortDescending();  // Sorting my list... Then reversing...

Sortable s = Sortable.natural();
s.sort();               // Natural sort
```

---

## 6.7 Default Methods in java.util Package

Java 8 added default methods to many existing interfaces:

| Interface | New Default Method | Purpose |
|-----------|-------------------|---------|
| `Iterable` | `forEach(Consumer)` | Loop with lambda |
| `Collection` | `stream()` | Create a Stream |
| `Collection` | `removeIf(Predicate)` | Remove matching elements |
| `List` | `sort(Comparator)` | Sort in-place |
| `List` | `replaceAll(UnaryOperator)` | Replace each element |
| `Map` | `forEach(BiConsumer)` | Loop key-value pairs |
| `Map` | `getOrDefault(key, default)` | Get value with fallback |
| `Map` | `putIfAbsent(key, value)` | Put only if key missing |
| `Map` | `computeIfAbsent(key, Function)` | Compute value if missing |

### Examples
```java
// forEach on list
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.forEach(System.out::println);

// removeIf
List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
nums.removeIf(n -> n % 2 == 0);
System.out.println(nums); // [1, 3, 5]

// replaceAll
List<String> words = new ArrayList<>(Arrays.asList("hello", "world"));
words.replaceAll(String::toUpperCase);
System.out.println(words); // [HELLO, WORLD]

// Map getOrDefault
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 90);
System.out.println(scores.getOrDefault("Bob", 0)); // 0
```

---

## 6.8 Key Differences

| Feature | Default Method | Static Method | Abstract Method |
|---------|---------------|---------------|----------------|
| Has body? | ✅ Yes | ✅ Yes | ❌ No |
| Can be overridden? | ✅ Yes | ❌ No | ✅ Must be |
| Called on? | Object | Interface name | Object |
| Inherited? | ✅ Yes | ❌ No | ✅ Yes |

---

## ✅ Checklist

- [ ] I understand why default methods were added
- [ ] I can write default and static methods in interfaces
- [ ] I can resolve the diamond problem
- [ ] I know the resolution rules (class > interface > super-interface)
- [ ] I know which JDK interfaces got new default methods

**Next → [07_Method_References.md](./07_Method_References.md)**
