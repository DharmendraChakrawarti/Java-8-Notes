# Java Learning Roadmap – Detailed Notes

---

## Phase 1: Core Java (Beginner)

### 1️⃣ Java Basics

- **Introduction to Java** – Java is a high‑level, object‑oriented programming language that is platform‑independent because compiled byte‑code runs on the Java Virtual Machine (JVM).
- **JDK, JRE, JVM** –
  - **JDK (Java Development Kit)** – Provides the compiler (`javac`), debugger, and other tools required to develop Java applications.
  - **JRE (Java Runtime Environment)** – Contains the JVM and core class libraries needed to *run* Java programs.
  - **JVM (Java Virtual Machine)** – Executes the compiled byte‑code on any operating system, providing Java’s “write once, run anywhere” capability.
- **Java Program Structure** – Every Java program must contain a class with a `main` method that serves as the entry point:
  ```java
  public class Main {
      public static void main(String[] args) {
          System.out.println("Hello, World!");
      }
  }
  ```
- **Compilation Process** – Source file `Main.java` is compiled with `javac` to produce `Main.class`. Run the program with `java Main` which invokes the JVM to execute the byte‑code.
- **Variables & Data Types** –
  - **Primitive types**: `byte, short, int, long, float, double, char, boolean`. They store raw values and have a fixed size.
  - **Reference types**: Objects such as `String`, arrays, or user‑defined classes. The variable holds a reference (memory address) to the actual object.
- **Type Casting** – Converting one type to another. Implicit (widening) casts happen automatically, while explicit (narrowing) casts require a cast operator.
  ```java
  int i = 10;          // int
  long l = i;          // implicit widening
  long big = 100L;
  int narrowed = (int) big; // explicit narrowing
  ```
- **Operators** – Arithmetic (`+ - * / %`), relational (`> < == !=`), logical (`&& || !`), and bitwise (`& | ^ ~ << >>`). They manipulate values and form expressions.
- **User Input (Scanner)** – `Scanner` reads input from the console easily.
  ```java
  import java.util.Scanner;
  
  Scanner sc = new Scanner(System.in);
  System.out.print("Enter your name: ");
  String name = sc.nextLine(); // reads an entire line
  System.out.println("Hello, " + name);
  sc.close();
  ```
- **Comments** – Documentation inside code that the compiler ignores.
  - Single‑line: `// comment`
  - Multi‑line: `/* comment */`
- **Keywords & Identifiers** – Reserved words (`class`, `static`, `if`, etc.) cannot be used as names. Identifiers must start with a letter, `$`, or `_` and can contain digits thereafter.

### 2️⃣ Control Statements

| Statement | Definition & Example |
|-----------|----------------------|
| `if` | Executes a block when a condition is true.
```java
if (x > 0) System.out.println("positive");
``` |
| `if‑else` | Chooses between two blocks based on a condition.
```java
if (x > 0) System.out.println("positive");
else System.out.println("non‑positive");
``` |
| `nested if` | An `if` inside another `if` for multiple conditions.
```java
if (a) {
    if (b) System.out.println("a and b");
}
``` |
| `switch` | Selects a block based on a variable's value (better than many `if‑else`).
```java
switch(day) {
    case 1: System.out.println("Mon"); break;
    case 2: System.out.println("Tue"); break;
    default: System.out.println("Other");
}
``` |
| `for` | Classic loop with init, condition, and update.
```java
for (int i = 0; i < 5; i++) System.out.println(i);
``` |
| `while` | Repeats while a condition remains true.
```java
while (count < 10) {
    System.out.println(count);
    count++;
}
``` |
| `do‑while` | Executes at least once, then repeats while condition is true.
```java
do {
    System.out.println(count);
    count++;
} while (count < 10);
``` |
| Enhanced `for` (for‑each) | Simplifies iteration over arrays or collections.
```java
for (String s : list) System.out.println(s);
``` |
| `break` / `continue` | `break` exits the nearest loop; `continue` skips to the next iteration.

### 3️⃣ Methods

```java
public class MathUtil {
    // Simple addition – method declaration
    public static int add(int a, int b) { return a + b; }

    // Overloaded addition – same name, different parameters
    public static int add(int a, int b, int c) { return a + b + c; }

    // Varargs – accept any number of ints
    public static int sum(int... numbers) {
        int total = 0;
        for (int n : numbers) total += n;
        return total;
    }

    // Recursive factorial example
    public static long fact(int n) {
        if (n <= 1) return 1; // base case
        return n * fact(n - 1); // recursive call
    }
}
```
- **Method Declaration** – Consists of an access modifier (`public`), optional `static`, a return type, method name, and parameter list.
- **Calling** – `int result = MathUtil.add(3, 4);`
- **Method Overloading** – Multiple methods with the same name but different parameter signatures; the compiler chooses based on arguments.
- **Varargs (`int... args`)** – Allows a method to accept a variable number of arguments; internally treated as an array.
- **Recursion** – A method that calls itself; must have a terminating condition to avoid infinite loops.

### 4️⃣ Arrays

```java
// 1‑D array – a simple list of values
int[] nums = {1, 2, 3, 4, 5};

// 2‑D array – matrix representation
int[][] matrix = {{1, 2}, {3, 4}};

// Traversal using enhanced for-loop
for (int n : nums) System.out.println(n);

// Copying an array efficiently
int[] copy = new int[nums.length];
System.arraycopy(nums, 0, copy, 0, nums.length);

// Sorting with built‑in utility (ascending order)
java.util.Arrays.sort(nums);

// Binary search (requires sorted array)
int idx = java.util.Arrays.binarySearch(nums, 3); // returns index of value 3
```
- **Array Operations** – `length` property, `clone()`, `Arrays.fill(array, value)`, `Arrays.copyOf`.
- **Why Arrays?** – Fixed size, contiguous memory, fast index access.

### 5️⃣ Strings

```java
String s = "Hello";                     // immutable literal
StringBuilder sb = new StringBuilder(); // mutable, ideal for concatenation in loops
sb.append("Hello");
sb.append(' ');
sb.append("World");
System.out.println(sb.toString()); // prints "Hello World"

// Common String methods
int len = s.length();
char c = s.charAt(0);
String sub = s.substring(1, 4); // "ell"
String upper = s.toUpperCase();
boolean contains = s.contains("ell");
```
- **String Pool** – JVM stores string literals in a shared pool; identical literals refer to the same object, allowing memory reuse.
- **String vs StringBuilder vs StringBuffer** – `String` is immutable (creates new objects on modification). `StringBuilder` is mutable and **not** thread‑safe (faster). `StringBuffer` is similar but synchronized for safe use in multithreaded contexts.
- **When to use which?** – Use `String` for fixed text, `StringBuilder` for intensive concatenation in a single thread, `StringBuffer` when multiple threads modify the same buffer.

---

## Phase 2: Object‑Oriented Programming (Must‑Know)

### 6️⃣ Classes & Objects

```java
public class Person {
    // Instance variables – each object has its own copy
    private String name;
    private int age;

    // Static variable – shared across all Person objects
    private static int population = 0;

    // Constructor – runs when a new object is instantiated
    public Person(String name, int age) {
        this.name = name; // 'this' refers to the current object
        this.age = age;
        population++; // increment static counter
    }

    // Instance method – behavior of the object
    public void introduce() {
        System.out.println("Hi, I am " + name + ", " + age + " years old.");
    }
}
```
- **Instance Variables** – Belong to each object (different values per object).
- **Static Variables** – Belong to the class; a single copy shared by all instances.
- **Local Variables** – Declared within a method; exist only during method execution.
- **Constructors** – Special methods without a return type used to initialise new objects.
- **`this` Keyword** – References the current object, useful to disambiguate instance fields from parameters.

### 7️⃣ OOP Concepts

| Concept | Definition | Example |
|---------|------------|---------|
| **Encapsulation** | Bundles data (fields) and methods together, restricting direct access to internal state. | Private fields + public getters/setters. |
| **Inheritance** | Enables a new class (`subclass`) to acquire properties and behaviours of an existing class (`superclass`). | `class Dog extends Animal {}` |
| **Polymorphism** | Allows a single interface to represent different underlying forms (e.g., method overriding). | `Shape s = new Circle(); s.draw();` |
| **Abstraction** | Hides complex implementation details, exposing only essential features via abstract classes or interfaces. | `abstract class Vehicle { abstract void start(); }` |
| **Interface** | Pure abstract contract that a class can implement, allowing multiple inheritance of type. | `interface Drivable { void drive(); }` |

### 8️⃣ Keywords

- `this` – Current object reference.
- `super` – Reference to the immediate superclass, useful to call overridden methods or constructors.
- `final` – Marks a class that cannot be subclassed, a method that cannot be overridden, or a variable whose value cannot change.
- `static` – Belongs to the class, not to any instance.
- `abstract` – Declares a class or method without implementation (must be subclassed).
- `synchronized` – Ensures that only one thread can execute a method or block at a time.
- `volatile` – Guarantees visibility of changes to variables across threads.
- `transient` – Excludes a field from default serialization.

### 9️⃣ Access Modifiers

| Modifier | Visibility |
|----------|------------|
| `public` | Accessible from any other class.
| `protected` | Accessible within the same package and subclasses (even in other packages).
| `private` | Accessible only within the declaring class.
| *default* (package‑private) | Accessible only within the same package.

---

## Phase 3: Advanced Core Java

### 🔟 Exception Handling

```java
try {
    int result = 10 / 0; // throws ArithmeticException (unchecked)
} catch (ArithmeticException e) {
    System.err.println("Cannot divide by zero!");
} finally {
    System.out.println("Cleanup always runs.");
}

// Custom checked exception example
class AgeException extends Exception {
    public AgeException(String msg) { super(msg); }
}

void checkAge(int age) throws AgeException {
    if (age < 18) {
        throw new AgeException("Age must be 18 or older.");
    }
    System.out.println("Welcome!");
}
```
- **Checked Exceptions** – Must be declared in the method signature (`throws`) or caught (`try‑catch`). Examples: `IOException`, `SQLException`.
- **Unchecked Exceptions** – Subclasses of `RuntimeException`; the compiler does not force handling. Examples: `NullPointerException`, `ArithmeticException`.
- **`throw`** – Explicitly raises an exception.
- **`throws`** – Declares that a method may propagate an exception to its caller.
- **`finally`** – Executes regardless of whether an exception was thrown, typically for resource cleanup.

### 1️⃣1️⃣ Collections Framework ⭐⭐⭐

```java
// List – ordered, allows duplicate elements
List<String> list = new ArrayList<>();
list.add("Alice");
list.add("Bob");
list.add("Alice"); // duplicate is permitted

// Set – unordered, **no** duplicates
Set<Integer> set = new HashSet<>();
set.add(1);
set.add(1); // duplicate ignored

// Map – key‑value pairs, keys are unique
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 95);
map.put("Bob", 88);
```
- **Implementations** – `ArrayList`, `LinkedList`, `Vector`, `Stack`, `HashSet`, `LinkedHashSet`, `TreeSet`, `PriorityQueue`, `ArrayDeque`, `HashMap`, `LinkedHashMap`, `TreeMap`, `Hashtable`, `ConcurrentHashMap`.
- **Core Interfaces** – `Collection` (super‑interface of `List`, `Set`, `Queue`), `List`, `Set`, `Queue`, `Map`.
- **Choosing the Right Implementation** – Consider ordering, concurrency, and performance characteristics (e.g., `ArrayList` for fast random access, `LinkedList` for frequent insertions/removals, `HashSet` for constant‑time lookup, `TreeSet` for sorted order).

### 1️⃣2️⃣ Generics

```java
// Generic class – type parameter T can be any reference type
class Box<T> {
    private T value;
    Box(T v) { this.value = v; }
    T get() { return value; }
}

Box<Integer> intBox = new Box<>(10);
Box<String> strBox = new Box<>("Hello");
```
- **Generic Methods** – Methods that declare their own type parameters.
  ```java
  public static <T> void printArray(T[] array) {
      for (T e : array) System.out.println(e);
  }
  ```
- **Wildcards** – Provide flexibility when the exact type is unknown.
  - `? extends Number` – Accepts any subclass of `Number` (read‑only).
  - `? super Integer` – Accepts `Integer` or any of its superclasses (write‑only).
- **Why Use Generics?** – Compile‑time type safety, eliminates the need for casting, and improves code readability.

### 1️⃣3️⃣ Multithreading

```java
// Extending Thread class
class MyThread extends Thread {
    public void run() { System.out.println("Running in MyThread"); }
}

// Implementing Runnable interface
class Task implements Runnable {
    public void run() { System.out.println("Running Task"); }
}

public static void main(String[] args) throws Exception {
    // Start thread via subclass
    new MyThread().start();

    // Start thread via Runnable
    new Thread(new Task()).start();

    // Executor framework – thread pool
    ExecutorService pool = Executors.newFixedThreadPool(2);
    Future<Integer> future = pool.submit(() -> {
        Thread.sleep(300);
        return 42;
    });
    System.out.println("Result from pool: " + future.get()); // 42
    pool.shutdown();
}
```
- **Thread Lifecycle** – `NEW → RUNNABLE → BLOCKED → TERMINATED`.
- **Synchronization** – `synchronized` keyword or explicit `Lock` objects prevent race conditions on shared mutable data.
- **Deadlock** – Occurs when two or more threads wait indefinitely for each other’s locks.
- **Callable & Future** – `Callable` returns a result and can throw checked exceptions; `Future` represents the pending result.

### 1️⃣4️⃣ File Handling

```java
// Write text to a file (try‑with‑resources automatically closes the writer)
try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
    bw.write("Hello, file!");
}

// Read text from a file line by line
try (BufferedReader br = new BufferedReader(new FileReader("output.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}

// Object serialization – convert an object to a byte stream
class Person implements Serializable {
    private String name;
    private int age;
    Person(String n, int a) { name = n; age = a; }
}

// Serialize
try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"))) {
    oos.writeObject(new Person("Alice", 30));
}

// Deserialize
try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"))) {
    Person p = (Person) ois.readObject();
    System.out.println(p);
}
```
- **Serialization vs. Deserialization** – Serialization writes the state of an object to a persistent medium; deserialization reconstructs the object from that state.

### 1️⃣5️⃣ Wrapper Classes & Autoboxing

| Primitive | Wrapper |
|-----------|---------|
| `byte`    | `Byte`   |
| `short`   | `Short`  |
| `int`     | `Integer`|
| `long`    | `Long`   |
| `float`   | `Float`  |
| `double`  | `Double` |
| `char`    | `Character`|
| `boolean` | `Boolean` |

```java
Integer i = 5; // autoboxing from int to Integer
int j = i;      // auto‑unboxing back to int
System.out.println(Integer.sum(7, 3)); // static utility method
```
- **Why Wrappers?** – Needed when working with collections that require objects (e.g., `List<Integer>`).

### 1️⃣6️⃣ Enums

```java
public enum Day {
    MON, TUE, WED, THU, FRI, SAT, SUN
}

Day today = Day.FRI;
System.out.println("Today is " + today);
```
- Enums are a special kind of class that represents a fixed set of constants. They can have fields, constructors, and methods, making them far more powerful than plain constants.

### 1️⃣7️⃣ Annotations

```java
@Override // tells the compiler the method overrides a superclass method
public String toString() { return "Person"; }

@Deprecated // marks a method as outdated; IDE will warn when used
public void oldMethod() { }

@SuppressWarnings("unchecked") // suppresses unchecked cast warnings
List raw = new ArrayList();
```
- **Retention Policies** – `SOURCE` (discarded by compiler), `CLASS` (stored in bytecode), `RUNTIME` (available via reflection).
- **Target Elements** – `TYPE`, `METHOD`, `FIELD`, `PARAMETER`, etc.
- **Custom Annotation Example**
  ```java
  import java.lang.annotation.*;

  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.METHOD)
  public @interface LogExecution {
      String value() default ""; // optional attribute
  }

  class Service {
      @LogExecution("serviceMethod")
      public void doWork() { System.out.println("working..."); }
  }
  ```

---

### 📚 How to Use This Roadmap
1. **Read each section** – Understand the core concept and its purpose.
2. **Run the code snippets** – Copy them into a `.java` file, compile, and execute to see the behavior.
3. **Experiment** – Modify values, add more cases, combine concepts (e.g., use collections inside methods).
4. **Progression** – Master Phase 1 before moving to Phase 2, then to Phase 3.
5. **Practice** – After each phase, attempt interview‑style questions to solidify knowledge.

Happy coding! 🎉
