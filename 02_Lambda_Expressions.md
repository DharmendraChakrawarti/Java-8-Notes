# 2. Lambda Expressions

> Lambda expressions are the **biggest feature** of Java 8. They let you write shorter, cleaner code by treating functions as values.

---

## 🧠 Theory – Why Lambda Expressions?

### The Problem Before Java 8
Before Java 8, if you wanted to pass a **behavior** (a piece of code) to a method, you had to create a whole class or an anonymous class. It was like writing a full letter just to say "Hello" – too much effort for a small task.

### Real-Life Analogy 🍕
Imagine you order food on **Zomato/Swiggy**:

- **Old Way (Before Lambda):** You write a formal letter to the restaurant, put it in an envelope, add a stamp, go to the post office, and mail it. The letter says: "Please deliver one Paneer Butter Masala to my address." → Too much work for a simple order!

- **New Way (Lambda):** You just open the app and tap "Order Now" → Quick, clean, same result!

**Lambda does the same thing for code** – it removes all the unnecessary boilerplate and keeps only the important part.

### Another Analogy – TV Remote 📺
Think of a **TV remote**:
- Each **button** = a small function (change channel, increase volume, mute)
- You don't need to know the **internal wiring** – you just press the button and it works
- Lambda is like assigning a **new action to a button** in one line: *"When I press this button, do this action"*

```
Button → Action
(parameter) → { what to do }
```

### One More Analogy – WhatsApp Quick Reply 💬
Think about replying to a message:
- **Old Way:** Open laptop → Open email app → Click compose → Type "To:", "Subject:", "Body:" → Click Send → Just to say "OK" 😩
- **Lambda Way:** Just type "OK" on WhatsApp and hit send ✅

Lambda removes the ceremony and keeps only the **essence** of what you want to say.

### In Simple Words
| Concept | Explanation |
|---------|------------|
| **What** | A short, nameless function (also called an anonymous function) |
| **Why** | To reduce boilerplate code and write cleaner programs |
| **When** | When you need to pass a small piece of behavior (like filtering, sorting, or processing) |
| **Where** | Works only with **functional interfaces** (interfaces with 1 abstract method) |
| **How** | `(inputs) -> { what to do with inputs }` |

### How Lambda Works Internally
1. Java sees a lambda expression
2. It checks which **functional interface** matches the lambda's signature
3. It automatically creates an **object** of that interface behind the scenes
4. The lambda body becomes the implementation of the interface's abstract method

> 🔑 **Key Insight**: Lambda doesn't create a new type – it's just a cleaner way to implement an existing interface.

### Behind The Scenes – Anonymous Class vs Lambda

Many beginners think lambda is just *syntactic sugar* for anonymous inner classes. **That's not fully true!**

| Aspect | Anonymous Inner Class | Lambda Expression |
|--------|----------------------|-------------------|
| **Bytecode** | Generates a separate `.class` file | Uses `invokedynamic` instruction (no extra class file) |
| **`this` keyword** | Refers to the anonymous class instance | Refers to the **enclosing class** instance |
| **Performance** | Slightly slower (class loading overhead) | Slightly faster (no extra class to load) |
| **Memory** | Creates a new object every time | JVM may cache and reuse the lambda instance |
| **Readability** | Verbose | Concise |

```java
// 'this' difference example
public class ThisDemo {
    String name = "Outer";

    void test() {
        // Anonymous class – 'this' = anonymous class object
        Runnable r1 = new Runnable() {
            String name = "Anonymous";
            @Override
            public void run() {
                System.out.println(this.name); // prints "Anonymous"
            }
        };

        // Lambda – 'this' = enclosing ThisDemo object
        Runnable r2 = () -> {
            System.out.println(this.name); // prints "Outer"
        };

        r1.run(); // Anonymous
        r2.run(); // Outer
    }
}
```

---

## 2.1 What is a Lambda?

A **lambda expression** is a short way to write an **anonymous function** (a function without a name).

### Easy Definition 📝
> **Lambda = a block of code that you can pass around and execute later, written in the shortest possible way.**

Think of it as: *"Here are the inputs → here's what I want to do with them"*

### Syntax
```
(parameters) -> { body }
```

The **three parts** of a lambda:
```
(int a, int b)  ->  { return a + b; }
─────┬──────    ─┬─  ──────┬──────
 Parameters    Arrow    Body (logic)
              Operator
```

### All Syntax Variations (with Examples)

```java
// 1️⃣ No parameters
() -> System.out.println("Hello!")

// 2️⃣ One parameter (parentheses optional)
x -> x * 2
(x) -> x * 2          // same thing, parentheses optional

// 3️⃣ Two parameters (parentheses required)
(a, b) -> a + b

// 4️⃣ With explicit types
(int a, int b) -> a + b

// 5️⃣ Multi-line body (curly braces + return required)
(a, b) -> {
    int sum = a + b;
    System.out.println("Sum = " + sum);
    return sum;
}

// 6️⃣ No parameter, no return
() -> System.out.println("Just do this!")

// 7️⃣ One parameter, no return
name -> System.out.println("Hello " + name)
```

### Simple Rules (Quick Reference Table)
| Rule | Example |
|------|---------|
| No parameters – use empty `()` | `() -> System.out.println("Hi")` |
| One parameter – no parentheses needed | `x -> x * 2` |
| Multiple parameters – use parentheses | `(a, b) -> a + b` |
| One statement – no curly braces needed | `x -> x * 2` |
| Multiple statements – use curly braces + `return` | `(a, b) -> { int sum = a + b; return sum; }` |
| Types are optional (Java infers them) | `(a, b) -> a + b` works like `(int a, int b) -> a + b` |
| If you write types, write for ALL params | ❌ `(int a, b)` → ✅ `(int a, int b)` |

---

## 2.2 Before vs After Lambda – Detailed Comparison

### Example 1: Sorting a List

**OLD way (Anonymous class) – 8 lines:**
```java
List<String> names = Arrays.asList("Charlie", "Alice", "Bob");

Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.compareTo(b);
    }
});

System.out.println(names); // [Alice, Bob, Charlie]
```

**NEW way (Lambda) – 1 line:**
```java
List<String> names = Arrays.asList("Charlie", "Alice", "Bob");

Collections.sort(names, (a, b) -> a.compareTo(b));

System.out.println(names); // [Alice, Bob, Charlie]
```

> 🔑 **Notice**: 8 lines reduced to 1 line! Same result, much cleaner.

### Example 2: Button Click Handler (JavaFX/Swing concept)

**OLD way:**
```java
button.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Button clicked!");
    }
});
```

**Lambda way:**
```java
button.addActionListener(e -> System.out.println("Button clicked!"));
```

### Example 3: Filtering a List

**OLD way:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> evenNumbers = new ArrayList<>();

for (Integer n : numbers) {
    if (n % 2 == 0) {
        evenNumbers.add(n);
    }
}
System.out.println(evenNumbers); // [2, 4, 6, 8, 10]
```

**Lambda + Stream way:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

System.out.println(evenNumbers); // [2, 4, 6, 8, 10]
```

---

## 2.3 Using Lambda with `forEach`

### Old way
```java
List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");

for (String fruit : fruits) {
    System.out.println(fruit);
}
```

### Lambda way
```java
List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");

fruits.forEach(fruit -> System.out.println(fruit));
```

### Even shorter with Method Reference
```java
fruits.forEach(System.out::println);
```

### forEach with Map
```java
Map<String, Integer> ages = new HashMap<>();
ages.put("Alice", 25);
ages.put("Bob", 30);
ages.put("Charlie", 35);

// Lambda with Map – both key and value
ages.forEach((name, age) -> System.out.println(name + " is " + age + " years old"));

// Output:
// Alice is 25 years old
// Bob is 30 years old
// Charlie is 35 years old
```

---

## 2.4 Lambda with Runnable (Threading)

### Old way
```java
Thread t = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Running in thread!");
    }
});
t.start();
```

### Lambda way
```java
Thread t = new Thread(() -> System.out.println("Running in thread!"));
t.start();
```

### Multi-line thread body
```java
Thread t = new Thread(() -> {
    System.out.println("Thread started...");
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.println("Thread finished!");
});
t.start();
```

---

## 2.5 Lambda with Comparator

```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 9);

// Sort ascending
numbers.sort((a, b) -> a - b);
System.out.println(numbers); // [1, 2, 5, 8, 9]

// Sort descending
numbers.sort((a, b) -> b - a);
System.out.println(numbers); // [9, 8, 5, 2, 1]
```

### Sorting Objects by a Field
```java
class Student {
    String name;
    int marks;

    Student(String name, int marks) {
        this.name = name;
        this.marks = marks;
    }

    @Override
    public String toString() {
        return name + "(" + marks + ")";
    }
}

List<Student> students = Arrays.asList(
    new Student("Alice", 85),
    new Student("Bob", 92),
    new Student("Charlie", 78)
);

// Sort by marks (ascending)
students.sort((s1, s2) -> s1.marks - s2.marks);
System.out.println(students); // [Charlie(78), Alice(85), Bob(92)]

// Sort by name (alphabetical)
students.sort((s1, s2) -> s1.name.compareTo(s2.name));
System.out.println(students); // [Alice(85), Bob(92), Charlie(78)]

// Even cleaner using Comparator.comparing()
students.sort(Comparator.comparing(s -> s.marks));
students.sort(Comparator.comparing(s -> s.name));
```

---

## 2.6 Custom Functional Interface with Lambda

You can create your own interface and use it with a lambda:

```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

public class LambdaDemo {
    public static void main(String[] args) {

        MathOperation add      = (a, b) -> a + b;
        MathOperation subtract = (a, b) -> a - b;
        MathOperation multiply = (a, b) -> a * b;
        MathOperation divide   = (a, b) -> a / b;

        System.out.println("5 + 3 = " + add.operate(5, 3));       // 8
        System.out.println("5 - 3 = " + subtract.operate(5, 3));   // 2
        System.out.println("5 * 3 = " + multiply.operate(5, 3));   // 15
        System.out.println("10 / 2 = " + divide.operate(10, 2));   // 5
    }
}
```

### Another Custom Example – String Processor
```java
@FunctionalInterface
interface StringProcessor {
    String process(String input);
}

public class StringDemo {
    public static void main(String[] args) {

        StringProcessor toUpper   = s -> s.toUpperCase();
        StringProcessor toLower   = s -> s.toLowerCase();
        StringProcessor addEmoji  = s -> s + " 🎉";
        StringProcessor reverse   = s -> new StringBuilder(s).reverse().toString();

        System.out.println(toUpper.process("hello"));    // HELLO
        System.out.println(toLower.process("WORLD"));    // world
        System.out.println(addEmoji.process("Done"));    // Done 🎉
        System.out.println(reverse.process("Java"));     // avaJ
    }
}
```

### Passing Lambda as a Method Argument
```java
@FunctionalInterface
interface Greeting {
    String greet(String name);
}

public class GreetingDemo {

    // Method that takes a lambda (functional interface) as parameter
    static void printGreeting(Greeting g, String name) {
        System.out.println(g.greet(name));
    }

    public static void main(String[] args) {
        printGreeting(name -> "Hello, " + name + "!", "Alice");
        // Output: Hello, Alice!

        printGreeting(name -> "Namaste, " + name + " 🙏", "Dharmendra");
        // Output: Namaste, Dharmendra 🙏

        printGreeting(name -> "Hey " + name + ", welcome!", "Bob");
        // Output: Hey Bob, welcome!
    }
}
```

---

## 2.7 Lambda with Built-in Functional Interfaces

Java 8 provides ready-made functional interfaces in `java.util.function` package. You don't need to create your own every time!

| Interface | Method | Input → Output | Example |
|-----------|--------|----------------|---------|
| `Predicate<T>` | `test(T)` | `T → boolean` | Check if a number is even |
| `Function<T,R>` | `apply(T)` | `T → R` | Convert String to its length |
| `Consumer<T>` | `accept(T)` | `T → void` | Print a value |
| `Supplier<T>` | `get()` | `() → T` | Generate a random number |
| `UnaryOperator<T>` | `apply(T)` | `T → T` | Double a number |
| `BinaryOperator<T>` | `apply(T,T)` | `(T, T) → T` | Add two numbers |

### Predicate – Check a condition (returns true/false)
```java
import java.util.function.Predicate;

Predicate<Integer> isEven = n -> n % 2 == 0;
Predicate<String> isLong = s -> s.length() > 5;

System.out.println(isEven.test(4));        // true
System.out.println(isEven.test(7));        // false
System.out.println(isLong.test("Hello"));  // false
System.out.println(isLong.test("Lambda")); // true

// Combining Predicates
Predicate<Integer> isPositive = n -> n > 0;
Predicate<Integer> isPositiveAndEven = isPositive.and(isEven);

System.out.println(isPositiveAndEven.test(4));  // true
System.out.println(isPositiveAndEven.test(-4)); // false
System.out.println(isPositiveAndEven.test(3));  // false
```

### Function – Transform input to output
```java
import java.util.function.Function;

Function<String, Integer> strLength = s -> s.length();
Function<Integer, Integer> doubleIt = n -> n * 2;

System.out.println(strLength.apply("Lambda"));  // 6
System.out.println(doubleIt.apply(5));          // 10

// Chaining Functions
Function<String, Integer> lengthThenDouble = strLength.andThen(doubleIt);
System.out.println(lengthThenDouble.apply("Hi")); // 4  (length=2, doubled=4)
```

### Consumer – Do something with input (no return)
```java
import java.util.function.Consumer;

Consumer<String> printUpper = s -> System.out.println(s.toUpperCase());
Consumer<String> printLower = s -> System.out.println(s.toLowerCase());

printUpper.accept("hello"); // HELLO
printLower.accept("WORLD"); // world

// Chaining Consumers
Consumer<String> printBoth = printUpper.andThen(printLower);
printBoth.accept("Java");
// JAVA
// java
```

### Supplier – Produce a value (no input)
```java
import java.util.function.Supplier;

Supplier<Double> randomNum = () -> Math.random();
Supplier<String> greeting  = () -> "Hello, World!";

System.out.println(randomNum.get()); // 0.7234... (random)
System.out.println(greeting.get());  // Hello, World!
```

---

## 2.8 Lambda Scope – Variable Capture

Lambdas can use variables from the **surrounding scope**, but those variables must be **effectively final** (not changed after initialization).

### What is "Effectively Final"?
A variable is **effectively final** if its value is **never changed** after being assigned, even if the `final` keyword is not written.

```java
String greeting = "Hello";  // effectively final (never changed)

Runnable r = () -> System.out.println(greeting + " World!");
r.run(); // Hello World!
```

❌ **This will NOT compile:**
```java
String greeting = "Hello";
greeting = "Hi";  // changed! NOT effectively final

Runnable r = () -> System.out.println(greeting); // COMPILE ERROR
```

### Why This Rule Exists?
Lambdas may execute **later** or in a **different thread**. If the variable could change, the lambda wouldn't know which value to use – leading to bugs. Java prevents this confusion by requiring effectively final variables.

### What You CAN Modify Inside Lambda

You **can** modify the contents of an object (like a list) – you just can't reassign the variable itself:

```java
List<String> names = new ArrayList<>();  // variable 'names' is effectively final

// ✅ OK – modifying the contents of the list (not reassigning 'names')
Runnable r = () -> {
    names.add("Alice");
    names.add("Bob");
};
r.run();
System.out.println(names); // [Alice, Bob]

// ❌ NOT OK – reassigning the variable
// names = new ArrayList<>();  // This would break the lambda!
```

### Scope Summary Table
| What | Can lambda access? | Can lambda modify? |
|------|-------------------|--------------------|
| Local variables | ✅ Yes (if effectively final) | ❌ No |
| Instance variables (`this.x`) | ✅ Yes | ✅ Yes |
| Static variables | ✅ Yes | ✅ Yes |
| Object contents (list/map items) | ✅ Yes | ✅ Yes |

---

## 2.9 Type Inference in Lambda

Java is smart enough to **figure out the types** of lambda parameters automatically. This is called **type inference**.

```java
// With explicit types
Comparator<String> c1 = (String a, String b) -> a.compareTo(b);

// Without types (Java infers them from the Comparator<String> context)
Comparator<String> c2 = (a, b) -> a.compareTo(b);

// Both are exactly the same!
```

### How Does Java Know the Types?

Java looks at the **target type** – the functional interface that the lambda is being assigned to:

```java
// Java sees: Predicate<String> → test(String) → so 's' must be String
Predicate<String> isEmpty = s -> s.isEmpty();

// Java sees: Function<String, Integer> → apply(String) → so 's' is String, return is Integer
Function<String, Integer> len = s -> s.length();

// Java sees: BiFunction<Integer, Integer, Integer> → so 'a' and 'b' are Integer
BiFunction<Integer, Integer, Integer> sum = (a, b) -> a + b;
```

---

## 2.10 Lambda vs Anonymous Class – When to Use What?

| Scenario | Use Lambda ✅ | Use Anonymous Class ✅ |
|----------|--------------|----------------------|
| Interface with **1 abstract method** | ✅ Perfect fit | Works, but verbose |
| Interface with **2+ abstract methods** | ❌ Not possible | ✅ Only option |
| Need to use **`this`** to refer to the inner object | ❌ `this` refers to outer class | ✅ `this` refers to inner class |
| Need **multiple methods** implemented | ❌ Not possible | ✅ Can override multiple methods |
| Simple, short logic | ✅ Best choice | Overkill |
| Need to maintain **state** (fields) | ❌ Lambdas can't have fields | ✅ Can have instance fields |

---

## 2.11 Common Mistakes ❌

| # | Mistake | Why It's Wrong | Fix |
|---|---------|---------------|-----|
| 1 | Forgetting `@FunctionalInterface` | Not an error but good practice – compiler will check it has exactly 1 abstract method | Always add `@FunctionalInterface` to your interfaces |
| 2 | Modifying outer variable inside lambda | Variables used in lambda must be effectively final | Use `AtomicInteger` or instance variables instead |
| 3 | Using lambda where interface has 2+ abstract methods | Lambda only works with functional interfaces (1 abstract method) | Use anonymous class instead |
| 4 | Writing `return` without curly braces | `x -> return x * 2` won't compile | Use `x -> x * 2` OR `x -> { return x * 2; }` |
| 5 | Mixing typed and untyped parameters | `(int a, b) -> a + b` won't compile | Use `(int a, int b)` OR `(a, b)` |
| 6 | Using lambda with generic raw types | Causes unchecked warnings | Always use generics: `Predicate<String>` not `Predicate` |

### Common Mistake Examples

```java
// ❌ Mistake: return without braces
// Predicate<Integer> p = n -> return n > 0;   // COMPILE ERROR

// ✅ Fix: remove 'return' (single expression, no braces)
Predicate<Integer> p1 = n -> n > 0;

// ✅ Fix: add braces + return + semicolon
Predicate<Integer> p2 = n -> { return n > 0; };


// ❌ Mistake: mixed types
// BiFunction<Integer, Integer, Integer> add = (int a, b) -> a + b;  // ERROR

// ✅ Fix: all typed or all untyped
BiFunction<Integer, Integer, Integer> add1 = (Integer a, Integer b) -> a + b;
BiFunction<Integer, Integer, Integer> add2 = (a, b) -> a + b;
```

---

## 2.12 Real-World Use Cases 🌍

### 1. Filtering a List of Employees
```java
List<Employee> employees = getEmployees(); // assume this returns a list

// Find employees with salary > 50000
List<Employee> highEarners = employees.stream()
    .filter(e -> e.getSalary() > 50000)
    .collect(Collectors.toList());

// Find employees in IT department
List<Employee> itTeam = employees.stream()
    .filter(e -> "IT".equals(e.getDepartment()))
    .collect(Collectors.toList());
```

### 2. Transforming Data
```java
List<String> names = Arrays.asList("alice", "bob", "charlie");

// Convert all to uppercase
List<String> upperNames = names.stream()
    .map(name -> name.toUpperCase())
    .collect(Collectors.toList());
// [ALICE, BOB, CHARLIE]
```

### 3. Event Handling (JavaFX)
```java
button.setOnAction(event -> {
    label.setText("Button was clicked!");
    System.out.println("Clicked at: " + LocalDateTime.now());
});
```

### 4. Scheduled Tasks
```java
ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);

scheduler.scheduleAtFixedRate(
    () -> System.out.println("Running every 5 seconds..."),
    0,    // initial delay
    5,    // period
    TimeUnit.SECONDS
);
```

### 5. Custom Sorting (Complex)
```java
List<String> cities = Arrays.asList("Mumbai", "Delhi", "Bangalore", "Chennai", "Pune");

// Sort by length, then alphabetically for same length
cities.sort(Comparator
    .comparingInt(String::length)
    .thenComparing(Comparator.naturalOrder())
);
System.out.println(cities);
// [Pune, Delhi, Mumbai, Chennai, Bangalore]
```

---

## 2.13 Lambda Best Practices ✨

| # | Practice | Example |
|---|---------|---------|
| 1 | Keep lambdas **short** (1-3 lines) | `n -> n % 2 == 0` ✅ |
| 2 | If logic is complex, **extract to a method** and use method reference | `this::processEmployee` ✅ |
| 3 | Use **meaningful parameter names** | `student -> student.getMarks()` ✅ not `s -> s.getMarks()` |
| 4 | Prefer **method references** when possible | `String::toUpperCase` instead of `s -> s.toUpperCase()` |
| 5 | Always add **`@FunctionalInterface`** to custom interfaces | Catches errors at compile time |
| 6 | Avoid side effects in lambdas | Don't modify external state inside `map()` or `filter()` |
| 7 | Use **built-in** functional interfaces before creating custom ones | `Predicate`, `Function`, `Consumer`, `Supplier` |

### When to Extract to a Method
```java
// ❌ Too complex for a lambda
employees.stream()
    .filter(e -> {
        boolean isActive = e.getStatus().equals("ACTIVE");
        boolean isHighEarner = e.getSalary() > 50000;
        boolean isInIT = e.getDepartment().equals("IT");
        return isActive && isHighEarner && isInIT;
    })
    .collect(Collectors.toList());

// ✅ Extract to a method – much cleaner
employees.stream()
    .filter(this::isEligibleEmployee)
    .collect(Collectors.toList());

// The method
private boolean isEligibleEmployee(Employee e) {
    boolean isActive = e.getStatus().equals("ACTIVE");
    boolean isHighEarner = e.getSalary() > 50000;
    boolean isInIT = e.getDepartment().equals("IT");
    return isActive && isHighEarner && isInIT;
}
```

---

## 2.14 Practice Exercises 📝

Try to solve these on your own before looking at the answers!

### Exercise 1: Write a lambda for each
```java
// a) Check if a number is positive
Predicate<Integer> isPositive = ____;

// b) Convert a string to its length
Function<String, Integer> length = ____;

// c) Print a message with "LOG: " prefix
Consumer<String> logger = ____;

// d) Generate current date
Supplier<LocalDate> today = ____;
```

<details>
<summary>👉 Click to see answers</summary>

```java
Predicate<Integer> isPositive = n -> n > 0;
Function<String, Integer> length = s -> s.length();
Consumer<String> logger = msg -> System.out.println("LOG: " + msg);
Supplier<LocalDate> today = () -> LocalDate.now();
```
</details>

### Exercise 2: Sort a list of strings by length
```java
List<String> words = Arrays.asList("elephant", "cat", "butterfly", "dog", "ant");
// Sort by length (shortest first)
// Your code here: ____
```

<details>
<summary>👉 Click to see answer</summary>

```java
words.sort((a, b) -> a.length() - b.length());
// OR
words.sort(Comparator.comparingInt(String::length));
// Result: [cat, dog, ant, elephant, butterfly]
```
</details>

### Exercise 3: Create a custom functional interface
```java
// Create an interface 'Validator<T>' with method: boolean validate(T t)
// Use it to validate:
//   - Email contains '@'
//   - Password length >= 8
//   - Age is between 18 and 60
```

<details>
<summary>👉 Click to see answer</summary>

```java
@FunctionalInterface
interface Validator<T> {
    boolean validate(T t);
}

Validator<String> emailValidator = email -> email.contains("@");
Validator<String> passwordValidator = pwd -> pwd.length() >= 8;
Validator<Integer> ageValidator = age -> age >= 18 && age <= 60;

System.out.println(emailValidator.validate("test@mail.com")); // true
System.out.println(passwordValidator.validate("12345"));      // false
System.out.println(ageValidator.validate(25));                  // true
```
</details>

---

## ✅ Key Takeaways

1. **Lambda = short anonymous function** → `(params) -> body`
2. Works only with **functional interfaces** (1 abstract method)
3. Reduces boilerplate code dramatically
4. Variables used in lambda must be **effectively final**
5. Java uses **type inference** – you usually don't need to write parameter types
6. **`this`** in lambda refers to the **enclosing class**, not the lambda itself
7. Internally uses **`invokedynamic`** – more efficient than anonymous classes
8. Use **built-in** interfaces (`Predicate`, `Function`, `Consumer`, `Supplier`) before creating custom ones
9. Keep lambdas **short**; extract complex logic to named methods
10. Lambda is the foundation for **Streams**, **Optional**, and modern Java patterns

---

## 🗺️ Lambda Cheat Sheet

```
┌─────────────────────────────────────────────────────────┐
│                  LAMBDA CHEAT SHEET                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  SYNTAX:     (params) -> expression                      │
│              (params) -> { statements; return val; }     │
│                                                          │
│  NO PARAM:   () -> "Hello"                               │
│  ONE PARAM:  x -> x * 2                                  │
│  TWO PARAM:  (a, b) -> a + b                             │
│                                                          │
│  COMMON INTERFACES:                                      │
│    Predicate<T>   : T -> boolean    (test)               │
│    Function<T,R>  : T -> R          (apply)              │
│    Consumer<T>    : T -> void       (accept)             │
│    Supplier<T>    : () -> T         (get)                │
│                                                          │
│  RULES:                                                  │
│    ✅ Only with functional interfaces                    │
│    ✅ Types are inferred                                 │
│    ✅ 'this' = enclosing class                           │
│    ❌ Can't modify local variables                       │
│    ❌ Can't throw checked exceptions (easily)            │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**Next → [03_Functional_Interfaces.md](./03_Functional_Interfaces.md)**
