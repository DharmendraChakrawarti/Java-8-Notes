# 3. Functional Interfaces

> A functional interface is an interface with **exactly one abstract method**. It is the base of lambda expressions and the Stream API.

---

## 🧠 Theory – Understanding Functional Interfaces

### What is it in Simple Words?
A functional interface is like a **contract** that says: *"I have exactly ONE job to do."* Since there's only one job, Java can figure out what you mean when you write a lambda.

### Real-Life Analogy – Kitchen Appliances 🍳
Think of kitchen appliances:
- **Mixer** → has ONE job: blend things. You give it ingredients (input), it blends them (action).
- **Toaster** → has ONE job: toast bread. You put bread in (input), toast comes out (output).
- **Microwave** → has ONE job: heat food. You put cold food in (input), hot food comes out (output).

Each appliance has **one main function**. Similarly, a functional interface has **one abstract method**.

Now think of this:
| Kitchen Tool | Functional Interface | Lambda |
|-------------|---------------------|--------|
| Mixer (blend things) | `Consumer<T>` (accept & use input) | `item -> blend(item)` |
| Toaster (bread → toast) | `Function<T, R>` (transform input to output) | `bread -> toast(bread)` |
| Food Tester (is it cooked?) | `Predicate<T>` (test & return yes/no) | `food -> food.isCooked()` |
| Vending Machine (get snack) | `Supplier<T>` (give output, no input) | `() -> new Snack()` |

### Real-Life Analogy – Office Roles 🏢
Think about office employees:
| Role | Does What | Like Which Interface? |
|------|----------|----------------------|
| **Security Guard** | Checks if you have an ID → YES/NO | `Predicate<T>` |
| **Translator** | Takes English → gives Hindi | `Function<T, R>` |
| **Announcer** | Takes a message → announces it (no return) | `Consumer<T>` |
| **Water Cooler** | You just press button → gives water (no input needed) | `Supplier<T>` |

### Why Only ONE Abstract Method?
Because **lambda expressions need exactly one method to implement**. If an interface had 2 abstract methods, Java wouldn't know which one the lambda is for!

```
// ✅ ONE abstract method = Lambda knows what to implement
interface Greet { void sayHi(String name); }
Greet g = name -> System.out.println("Hi " + name);

// ❌ TWO abstract methods = Lambda is confused! Which one?
interface BadInterface { 
    void method1();
    void method2();  // Lambda can't handle this!
}
```

---

## 3.1 What is a Functional Interface?

```java
@FunctionalInterface
interface Greeting {
    void sayHello(String name);  // only ONE abstract method
}
```

- `@FunctionalInterface` → Optional annotation but **recommended**. The compiler checks that the interface has exactly one abstract method.
- Can have **default** and **static** methods in addition.

### Using it with Lambda
```java
Greeting g = name -> System.out.println("Hello, " + name + "!");
g.sayHello("Dharmendra");  // Hello, Dharmendra!
```

---

## 3.2 Built-in Functional Interfaces (java.util.function)

Java 8 provides **43 built-in functional interfaces**. The 4 most important ones are:

| Interface | Method | Input → Output | Use Case |
|-----------|--------|---------------|----------|
| `Predicate<T>` | `test(T)` | `T → boolean` | Filtering, conditions |
| `Function<T, R>` | `apply(T)` | `T → R` | Transforming values |
| `Consumer<T>` | `accept(T)` | `T → void` | Performing actions |
| `Supplier<T>` | `get()` | `() → T` | Providing values |

---

## 3.3 Predicate\<T\> – Test a condition

Returns `true` or `false`.

```java
import java.util.function.Predicate;

Predicate<Integer> isEven = n -> n % 2 == 0;
Predicate<String> isLong = s -> s.length() > 5;

System.out.println(isEven.test(4));      // true
System.out.println(isEven.test(7));      // false
System.out.println(isLong.test("Java")); // false
System.out.println(isLong.test("JavaScript")); // true
```

### Chaining Predicates
```java
Predicate<Integer> isEven     = n -> n % 2 == 0;
Predicate<Integer> isPositive = n -> n > 0;

// AND – both must be true
Predicate<Integer> isEvenAndPositive = isEven.and(isPositive);
System.out.println(isEvenAndPositive.test(4));  // true
System.out.println(isEvenAndPositive.test(-4)); // false

// OR – at least one must be true
Predicate<Integer> isEvenOrPositive = isEven.or(isPositive);
System.out.println(isEvenOrPositive.test(3));   // true (positive)

// NEGATE – flip the result
Predicate<Integer> isOdd = isEven.negate();
System.out.println(isOdd.test(5)); // true
```

### Using Predicate with Stream
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);

List<Integer> evenNumbers = numbers.stream()
    .filter(isEven)
    .collect(Collectors.toList());

System.out.println(evenNumbers); // [2, 4, 6, 8]
```

---

## 3.4 Function\<T, R\> – Transform a value

Takes input of type `T`, returns output of type `R`.

```java
import java.util.function.Function;

Function<String, Integer> lengthOf = s -> s.length();
Function<Integer, Integer> doubleIt = n -> n * 2;

System.out.println(lengthOf.apply("Java"));  // 4
System.out.println(doubleIt.apply(5));        // 10
```

### Chaining Functions
```java
Function<String, Integer> length  = String::length;
Function<Integer, Integer> square = n -> n * n;

// andThen – first apply length, then square the result
Function<String, Integer> lengthThenSquare = length.andThen(square);
System.out.println(lengthThenSquare.apply("Java")); // 16  (4 * 4)

// compose – first apply square, then... wait, types don't match here
// compose is the reverse: square runs first, then length
// Useful when types align
```

### Using Function with Stream
```java
List<String> names = Arrays.asList("alice", "bob", "charlie");

List<String> upperNames = names.stream()
    .map(String::toUpperCase)   // Function<String, String>
    .collect(Collectors.toList());

System.out.println(upperNames); // [ALICE, BOB, CHARLIE]
```

---

## 3.5 Consumer\<T\> – Do something with a value

Takes input, returns **nothing** (void).

```java
import java.util.function.Consumer;

Consumer<String> print = s -> System.out.println(s);
Consumer<String> shout = s -> System.out.println(s.toUpperCase() + "!!!");

print.accept("hello");  // hello
shout.accept("hello");  // HELLO!!!
```

### Chaining Consumers
```java
Consumer<String> greet = s -> System.out.print("Hello, ");
Consumer<String> name  = s -> System.out.println(s + "!");

// andThen – run greet first, then name
Consumer<String> greetAndName = greet.andThen(name);
greetAndName.accept("Dharmendra"); // Hello, Dharmendra!
```

### Using Consumer with forEach
```java
List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");

fruits.forEach(fruit -> System.out.println("I love " + fruit));
// I love Apple
// I love Banana
// I love Cherry
```

---

## 3.6 Supplier\<T\> – Provide a value

Takes **no input**, returns a value.

```java
import java.util.function.Supplier;

Supplier<String> greeting  = () -> "Hello, World!";
Supplier<Double> randomNum = () -> Math.random();
Supplier<List<String>> emptyList = ArrayList::new;

System.out.println(greeting.get());    // Hello, World!
System.out.println(randomNum.get());   // 0.7346... (random)
System.out.println(emptyList.get());   // []
```

### Real-world Use – Lazy Initialization
```java
// Value is created only when get() is called
Supplier<Connection> dbConnection = () -> {
    System.out.println("Creating database connection...");
    return DriverManager.getConnection("jdbc:mysql://localhost/mydb");
};

// Connection is NOT created yet
// ...
// Connection is created NOW:
Connection conn = dbConnection.get();
```

---

## 3.7 Other Useful Functional Interfaces

| Interface | Method | Description |
|-----------|--------|-------------|
| `BiPredicate<T, U>` | `test(T, U)` | Two inputs → boolean |
| `BiFunction<T, U, R>` | `apply(T, U)` | Two inputs → one output |
| `BiConsumer<T, U>` | `accept(T, U)` | Two inputs → void |
| `UnaryOperator<T>` | `apply(T)` | Same type in and out (extends `Function<T,T>`) |
| `BinaryOperator<T>` | `apply(T, T)` | Two same-type inputs → same-type output |

### Examples
```java
// BiFunction – takes 2 inputs, returns 1 output
BiFunction<String, String, String> fullName = (first, last) -> first + " " + last;
System.out.println(fullName.apply("Dharmendra", "Kumar")); // Dharmendra Kumar

// UnaryOperator – input & output are same type
UnaryOperator<String> toUpper = s -> s.toUpperCase();
System.out.println(toUpper.apply("java")); // JAVA

// BinaryOperator – two same-type inputs, same-type output
BinaryOperator<Integer> max = (a, b) -> a > b ? a : b;
System.out.println(max.apply(10, 20)); // 20
```

---

## 3.8 Creating Your Own Functional Interface

```java
@FunctionalInterface
interface Converter<F, T> {
    T convert(F from);
}

// Usage
Converter<String, Integer> stringToInt = Integer::valueOf;
Converter<String, Double> stringToDouble = Double::valueOf;

System.out.println(stringToInt.convert("123"));    // 123
System.out.println(stringToDouble.convert("3.14")); // 3.14
```

---

## ✅ Quick Summary Table

| Interface | Input | Output | Example Lambda |
|-----------|-------|--------|---------------|
| `Predicate<T>` | T | boolean | `n -> n > 0` |
| `Function<T,R>` | T | R | `s -> s.length()` |
| `Consumer<T>` | T | void | `s -> System.out.println(s)` |
| `Supplier<T>` | none | T | `() -> "Hello"` |

---

## ✅ Checklist

- [ ] I can use all 4 core functional interfaces
- [ ] I can chain predicates with `and()`, `or()`, `negate()`
- [ ] I can chain functions with `andThen()` and `compose()`
- [ ] I can create my own `@FunctionalInterface`

**Next → [04_Stream_API.md](./04_Stream_API.md)**
