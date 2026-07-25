# Java Learning Roadmap

---

## Phase 1: Core Java (Beginner)

### 1️⃣ Java Basics

- **Introduction to Java** – A high‑level, object‑oriented programming language that runs on the Java Virtual Machine (JVM).
- **JDK, JRE, JVM** –
  - **JDK** (Java Development Kit): Tools for developing Java applications (compiler, debugger, etc.).
  - **JRE** (Java Runtime Environment): Runtime libraries + JVM to run Java programs.
  - **JVM** (Java Virtual Machine): Executes compiled byte‑code on any platform.
- **Java Program Structure** – Every Java program must contain a class with a `main` method:
  ```java
  public class Main {
      public static void main(String[] args) {
          // program entry point
          System.out.println("Hello, World!");
      }
  }
  ```
- **Compilation Process** – `javac Main.java` produces `Main.class`; run with `java Main`.
- **Variables & Data Types** –
  - Primitive types: `byte, short, int, long, float, double, char, boolean`.
  - Reference types: objects such as `String`, arrays, custom classes.
- **Type Casting** –
  ```java
  int i = 10;          // implicit (widening) cast to long
  long l = i;
  long big = 100L;
  int narrowed = (int) big; // explicit (narrowing) cast
  ```
- **Operators** – Arithmetic (`+ - * / %`), relational (`> < == !=`), logical (`&& || !`), bitwise (`& | ^ ~ << >>`).
- **User Input (Scanner)**
  ```java
  import java.util.Scanner;
  
  Scanner sc = new Scanner(System.in);
  System.out.print("Enter your name: ");
  String name = sc.nextLine(); // reads a full line
  System.out.println("Hello, " + name);
  sc.close();
  ```
- **Comments** – `// single line` and `/* multi‑line */`.
- **Keywords & Identifiers** – Reserved words (`class`, `static`, `if`…) cannot be used as identifiers. Identifiers must start with a letter, `_` or `$` and can contain digits.

### 2️⃣ Control Statements

| Statement | Syntax Example |
|-----------|----------------|
| `if` | `if (x > 0) System.out.println("positive");` |
| `if‑else` | `if (x > 0) … else …` |
| `nested if` | `if (a) { if (b) … }` |
| `switch` | ```java
switch(day) {
    case 1: System.out.println("Mon"); break;
    case 2: System.out.println("Tue"); break;
    default: System.out.println("Other");
}
``` |
| `for` loop | `for (int i = 0; i < 5; i++) System.out.println(i);` |
| `while` loop | `while (cond) { … }` |
| `do‑while` loop | `do { … } while (cond);` |
| Enhanced `for` (for‑each) | `for (String s : list) System.out.println(s);` |
| `break` / `continue` | Alters loop execution flow. |

### 3️⃣ Methods

```java
public class MathUtil {
    // Simple addition – method declaration
    public static int add(int a, int b) {
        return a + b; // return statement
    }

    // Overloaded method – same name, different params
    public static int add(int a, int b, int c) {
        return a + b + c;
    }

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
- **Method Declaration** – Visibility (`public`), `static`/instance, return type, name, parameter list.
- **Calling** – `int r = MathUtil.add(3,4);`
- **Overloading** – Same name, different parameter signatures.
- **Varargs** – `int... args` collects any number of arguments.
- **Recursion** – Function calls itself; must have a base case.

### 4️⃣ Arrays

```java
// 1‑D array
int[] nums = {1, 2, 3, 4, 5};

// 2‑D array (matrix)
int[][] matrix = {{1,2},{3,4}};

// Traversal – enhanced for loop
for (int n : nums) System.out.println(n);

// Copying – System.arraycopy
int[] copy = new int[nums.length];
System.arraycopy(nums, 0, copy, 0, nums.length);

// Sorting
java.util.Arrays.sort(nums);

// Binary search (requires sorted array)
int idx = java.util.Arrays.binarySearch(nums, 3); // returns index of value 3
```
- **Array Operations** – `length`, `clone()`, `Arrays.fill(array, value)`, `Arrays.copyOf`.

### 5️⃣ Strings

```java
String s = "Hello";                     // immutable literal
StringBuilder sb = new StringBuilder(); // mutable, fast for concatenation
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
- **String Pool** – JVM caches literal strings; `==` compares references, `equals()` compares content.
- **String vs StringBuilder vs StringBuffer** – `String` immutable, `StringBuilder` mutable (not thread‑safe), `StringBuffer` mutable and synchronized.

---

## Phase 2: Object‑Oriented Programming (Must‑Know)

### 6️⃣ Classes & Objects

```java
public class Person {
    // Instance variables (fields)
    private String name; // each object has its own name
    private int age;

    // Static variable – shared among all Person instances
    private static int population = 0;

    // Constructor – runs when a new object is created
    public Person(String name, int age) {
        this.name = name; // "this" refers to current object
        this.age = age;
        population++; // increment static counter
    }

    // Method – behavior of the object
    public void introduce() {
        System.out.println("Hi, I am " + name + ", " + age + " years old.");
    }
}
```
- **Instance Variables** – Belong to each object.
- **Static Variables** – Belong to the class itself.
- **Local Variables** – Declared inside methods.
- **Constructors** – Special methods used to initialise new objects.
- **`this` Keyword** – Reference to the current object instance.

### 7️⃣ OOP Concepts

| Concept | Definition | Quick Example |
|---------|------------|---------------|
| Encapsulation | Bundle data (fields) with methods; hide internals using access modifiers. | Private fields + public getters/setters. |
| Inheritance | Derive a new class from an existing class to reuse code. | `class Dog extends Animal {}` |
| Polymorphism | Same operation behaves differently based on the object type. | Method overriding, interface implementation. |
| Abstraction | Hide complex implementation, expose only essential features. | `abstract class Shape { abstract void draw(); }` |
| Interface | Pure abstract contract; a class can implement multiple interfaces. | `interface Drivable { void drive(); }` |

### 8️⃣ Keywords

- `this` – current object reference.
- `super` – reference to superclass members.
- `final` – cannot be overridden (methods) or subclassed (classes) or reassigned (variables).
- `static` – belongs to class, not instance.
- `abstract` – class or method without implementation.
- `synchronized` – ensures exclusive lock for thread safety.
- `volatile` – forces read/write directly to main memory (visibility across threads).
- `transient` – field ignored during serialization.

### 9️⃣ Access Modifiers

| Modifier | Visibility |
|----------|------------|
| `public` | Anywhere (any class, any package). |
| `protected` | Same package or subclasses (even in different packages). |
| `private` | Only within the declaring class. |
| (default) *package‑private* | Only within the same package. |

---

## Phase 3: Advanced Core Java

### 🔟 Exception Handling

```java
try {
    int result = 10 / 0; // ArithmeticException (unchecked)
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
- **Checked Exceptions** – Must be declared in method signature or caught (`IOException`).
- **Unchecked Exceptions** – Subclasses of `RuntimeException`; not required to be declared (`NullPointerException`).
- **`throw`** – Manually raise an exception.
- **`throws`** – Declare that a method may propagate an exception.

### 1️⃣1️⃣ Collections Framework ⭐⭐⭐

```java
// List – ordered, allows duplicates
List<String> list = new ArrayList<>();
list.add("Alice");
list.add("Bob");
list.add("Alice"); // duplicate allowed

// Set – no duplicates
Set<Integer> set = new HashSet<>();
set.add(1);
set.add(1); // duplicate ignored

// Map – key‑value pairs
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 95);
map.put("Bob", 88);
```
- **Implementations** – `ArrayList`, `LinkedList`, `Vector`, `Stack`, `HashSet`, `LinkedHashSet`, `TreeSet`, `PriorityQueue`, `ArrayDeque`, `HashMap`, `LinkedHashMap`, `TreeMap`, `Hashtable`, `ConcurrentHashMap`.
- **Core Interfaces** – `Collection`, `List`, `Set`, `Queue`, `Map`.

### 1️⃣2️⃣ Generics

```java
// Generic class example
class Box<T> {
    private T value;
    Box(T v) { this.value = v; }
    T get() { return value; }
}

Box<Integer> intBox = new Box<>(10);
Box<String> strBox = new Box<>("Hello");
```
- **Generic Methods** – Example:
  ```java
  public static <T> void printArray(T[] array) {
      for (T e : array) System.out.println(e);
  }
  ```
- **Wildcards** – `List<? extends Number>` (covariant), `List<? super Integer>` (contravariant).

### 1️⃣3️⃣ Multithreading

```java
// Extending Thread class
class MyThread extends Thread {
    public void run() {
        System.out.println("Running in MyThread");
    }
}

// Implementing Runnable interface
class Task implements Runnable {
    public void run() {
        System.out.println("Running Task");
    }
}

public static void main(String[] args) throws Exception {
    // Start thread by extending Thread
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
- **Synchronization** – `synchronized` keyword or `Lock` objects to avoid race conditions.
- **Deadlock** – When two threads hold locks the other needs.
- **Callable & Future** – Allows tasks to return results and throw checked exceptions.

### 1️⃣4️⃣ File Handling

```java
// Write text to a file using try‑with‑resources
try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
    bw.write("Hello, file!");
}

// Read text from a file
try (BufferedReader br = new BufferedReader(new FileReader("output.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}

// Object serialization example
import java.io.*;

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
- **Serialization** – Convert objects to a byte stream; **Deserialization** – Reconstruct objects from bytes.

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
Integer i = 10; // autoboxing from int to Integer
int j = i;      // auto‑unboxing back to int

int sum = Integer.sum(5, 7); // static utility method
```

### 1️⃣6️⃣ Enums

```java
public enum Day {
    MON, TUE, WED, THU, FRI, SAT, SUN
}

Day today = Day.FRI;
System.out.println("Today is " + today);
```
- Enums are implicitly `static` and `final`; can have fields, constructors, and methods.

### 1️⃣7️⃣ Annotations

```java
@Override // tells compiler we are overriding a method
public String toString() { return "Person"; }

@Deprecated // marks method as deprecated
public void oldMethod() { }

@SuppressWarnings("unchecked") // suppresses compiler warnings
List raw = new ArrayList();
```
- **Retention Policies** – `SOURCE`, `CLASS`, `RUNTIME`.
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
1. **Read each section** – Understand the concept and terminology.
2. **Run the snippets** – Copy them into a `.java` file, compile, and execute.
3. **Experiment** – Modify values, add new cases, combine multiple concepts.
4. **Progression** – Master Phase 1 before moving to Phase 2, then Phase 3.
5. **Practice questions** – After each phase, try interview‑style questions to solidify knowledge.

Happy coding! 🎉
