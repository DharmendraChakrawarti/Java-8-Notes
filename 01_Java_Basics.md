# 1. Java Basics & OOP Refresher

> Before jumping into Java 8 features, make sure your foundation is strong. This file covers the basics you need.

---

## 🧠 Theory – Why Java Basics Matter

### Real-Life Analogy – Building a House 🏠
Think of learning Java like **building a house**:
- **Data Types** = Building materials (bricks, cement, wood)
- **Variables** = Rooms (each room holds something)
- **Methods** = Workers (each worker does a specific job)
- **Classes** = Blueprint (the design plan for the house)
- **Objects** = The actual houses built from the blueprint
- **Inheritance** = A luxury house design based on a basic house plan (reuses the basic plan + adds more)

You can't build a house without materials and a plan. Similarly, you can't learn Java 8 without understanding these basics!

### OOP in Simple Words
| Pillar | Simple Meaning | Real-Life Example |
|--------|---------------|------------------|
| **Encapsulation** | Hide internal details, show only what's needed | ATM machine – you use buttons, you don't see the cash vault inside 🏧 |
| **Inheritance** | Child gets properties from parent | You inherit features from your parents – eye color, height 👨‍👩‍👦 |
| **Polymorphism** | Same action, different behaviors | "Open" – you open a door 🚪, open a book 📖, open an app 📱 – same word, different actions |
| **Abstraction** | Show only essential features | Car dashboard – you see speed, fuel, temperature. You don't see the engine internals 🚗 |

---

## 1.1 Data Types

Java has **two categories** of data types:

### Primitive Types
| Type | Size | Example |
|------|------|---------|
| `byte` | 1 byte | `byte b = 10;` |
| `short` | 2 bytes | `short s = 1000;` |
| `int` | 4 bytes | `int num = 50000;` |
| `long` | 8 bytes | `long l = 100000L;` |
| `float` | 4 bytes | `float f = 3.14f;` |
| `double` | 8 bytes | `double d = 3.14159;` |
| `char` | 2 bytes | `char c = 'A';` |
| `boolean` | 1 bit | `boolean flag = true;` |

### Reference Types
- `String`, `Arrays`, `Objects`, `Collections`, etc.

```java
String name = "Dharmendra";      // Reference type
int[] numbers = {1, 2, 3, 4};   // Array (reference type)
```

---

## 1.2 Control Flow

### if-else
```java
int age = 20;
if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}
```

### switch
```java
int day = 3;
switch (day) {
    case 1: System.out.println("Monday"); break;
    case 2: System.out.println("Tuesday"); break;
    case 3: System.out.println("Wednesday"); break;
    default: System.out.println("Other day");
}
```

### Loops
```java
// for loop
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

// while loop
int count = 0;
while (count < 3) {
    System.out.println("Count: " + count);
    count++;
}

// enhanced for loop (for-each)
int[] nums = {10, 20, 30};
for (int n : nums) {
    System.out.println(n);
}
```

---

## 1.3 Methods

A method is a block of code that performs a specific task.

```java
public class Calculator {

    // Method with parameters and return type
    public static int add(int a, int b) {
        return a + b;
    }

    // Method with no return (void)
    public static void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }

    public static void main(String[] args) {
        int result = add(5, 3);
        System.out.println("Sum = " + result);  // Sum = 8

        greet("Dharmendra");  // Hello, Dharmendra!
    }
}
```

---

## 1.4 Object-Oriented Programming (OOP)

Java is built on **4 pillars of OOP**:

### 1. Encapsulation – Hide internal details
```java
public class BankAccount {
    private double balance;  // private = hidden

    public double getBalance() {       // getter
        return balance;
    }

    public void deposit(double amount) { // setter-like
        if (amount > 0) {
            balance += amount;
        }
    }
}
```
> **Key idea**: Use `private` fields + `public` getters/setters to control access.

### 2. Inheritance – Reuse code from parent class
```java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks!");
    }
}

// Usage
Dog dog = new Dog();
dog.eat();  // inherited from Animal
dog.bark(); // own method
```

### 3. Polymorphism – One name, many forms
```java
class Shape {
    void draw() {
        System.out.println("Drawing a shape");
    }
}

class Circle extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing a circle ⭕");
    }
}

class Square extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing a square 🟥");
    }
}

// Usage – same method name, different behavior
Shape s1 = new Circle();
Shape s2 = new Square();
s1.draw();  // Drawing a circle ⭕
s2.draw();  // Drawing a square 🟥
```

### 4. Abstraction – Show only what's needed
```java
abstract class Vehicle {
    abstract void start();  // no body – child MUST implement

    void stop() {
        System.out.println("Vehicle stopped.");
    }
}

class Car extends Vehicle {
    @Override
    void start() {
        System.out.println("Car started with key 🔑");
    }
}
```

---

## 1.5 Exception Handling

```java
public class ExceptionDemo {
    public static void main(String[] args) {

        // try-catch-finally
        try {
            int result = 10 / 0;  // ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Error: Cannot divide by zero!");
        } finally {
            System.out.println("This always runs.");
        }

        // try-with-resources (auto-close)
        try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
            System.out.println(br.readLine());
        } catch (IOException e) {
            System.out.println("File not found!");
        }
    }
}
```

### Custom Exception
```java
class AgeException extends Exception {
    public AgeException(String message) {
        super(message);
    }
}

// Usage
public static void checkAge(int age) throws AgeException {
    if (age < 18) {
        throw new AgeException("Age must be 18 or above!");
    }
    System.out.println("Welcome!");
}
```

---

## 1.6 Collections Framework (Before Java 8)

### List – Ordered, allows duplicates
```java
List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
names.add("Alice"); // duplicate allowed
System.out.println(names); // [Alice, Bob, Alice]
```

### Set – No duplicates
```java
Set<String> cities = new HashSet<>();
cities.add("Delhi");
cities.add("Mumbai");
cities.add("Delhi"); // ignored
System.out.println(cities); // [Delhi, Mumbai]
```

### Map – Key-Value pairs
```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 95);
scores.put("Bob", 88);
System.out.println(scores.get("Alice")); // 95
```

### Iterating Collections (Old Way)
```java
// Using Iterator
Iterator<String> it = names.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}

// Using for-each
for (String name : names) {
    System.out.println(name);
}
```

---

## 1.7 String Handling

```java
String s1 = "Hello";
String s2 = "World";

// Concatenation
String s3 = s1 + " " + s2;   // "Hello World"

// Important methods
s1.length();          // 5
s1.charAt(0);         // 'H'
s1.substring(1, 3);   // "el"
s1.toUpperCase();     // "HELLO"
s1.equals("Hello");   // true
s1.contains("ell");   // true

// StringBuilder (mutable, faster for loops)
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(" World");
System.out.println(sb.toString()); // "Hello World"
```

---

## ✅ Checklist Before Moving On

- [ ] I can write a class with fields, constructor, and methods
- [ ] I understand inheritance and can override methods
- [ ] I know the difference between `ArrayList`, `HashSet`, and `HashMap`
- [ ] I can handle exceptions using try-catch
- [ ] I can iterate over a collection using for-each and Iterator

**Next → [02_Lambda_Expressions.md](./02_Lambda_Expressions.md)**
