# ☕ Java Complete Notes – From Beginner to Advanced

---

# 📗 Phase 1: Core Java (Beginner)

---

## 1. Java Basics

---

### 1.1 Introduction to Java

**Definition:** Java is a high-level, object-oriented, platform-independent programming language developed by **James Gosling** at **Sun Microsystems** in 1995 (now owned by Oracle).

**Why Java is popular:**
- **Platform Independent** – Write once, run anywhere (WORA). Java code runs on any OS that has a JVM.
- **Object-Oriented** – Everything in Java is based on objects and classes.
- **Secure** – No pointers, bytecode verification, and security manager.
- **Robust** – Strong memory management, exception handling, and garbage collection.
- **Multithreaded** – Supports running multiple tasks simultaneously.

**Real-life analogy:** Think of Java like a **universal remote** 🎮 – it works with any TV brand (Windows, Mac, Linux) without modification.

---

### 1.2 JDK, JRE, JVM

These three are the backbone of how Java programs are created and executed.

#### JDK (Java Development Kit)
**Definition:** JDK is a complete software development kit that provides everything you need to **develop** Java applications. It includes the compiler (`javac`), debugger, documentation tools, and the JRE.

**Simple meaning:** JDK = Tools to **write + compile + run** Java programs.

#### JRE (Java Runtime Environment)
**Definition:** JRE provides the libraries, JVM, and other components needed to **run** Java applications. It does NOT include development tools like the compiler.

**Simple meaning:** JRE = Tools to **run** Java programs only (no compiling).

#### JVM (Java Virtual Machine)
**Definition:** JVM is a virtual machine that executes Java bytecode. It converts the `.class` file (bytecode) into machine-specific code for the operating system.

**Simple meaning:** JVM = The **engine** that actually runs your Java program.

```
┌─────────────────────────────────┐
│           JDK                   │
│  ┌───────────────────────────┐  │
│  │         JRE               │  │
│  │  ┌─────────────────────┐  │  │
│  │  │       JVM           │  │  │
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
│  + javac (compiler)             │
│  + javadoc, jar, etc.           │
└─────────────────────────────────┘
```

---

### 1.3 Java Program Structure

**Definition:** Every Java program must have at least one class and a `main` method. The `main` method is the **entry point** – the first thing that runs when you execute a program.

```java
// File name must match the public class name: Main.java

public class Main {  // Class declaration

    // main method – entry point of the program
    public static void main(String[] args) {
        System.out.println("Hello, Java!");  // prints text to console
    }
}
```

**Breakdown of `public static void main(String[] args)`:**
- `public` – accessible from anywhere
- `static` – can be called without creating an object
- `void` – does not return any value
- `main` – name of the method (JVM looks for this)
- `String[] args` – accepts command-line arguments

---

### 1.4 Compilation Process

**Definition:** Java uses a **two-step** process to run code: first it compiles source code into bytecode, then the JVM interprets/executes that bytecode.

```
Step 1: Write code      →  Main.java  (source code)
Step 2: Compile          →  javac Main.java  →  Main.class  (bytecode)
Step 3: Run              →  java Main  →  JVM executes the bytecode
```

```java
// Terminal commands:
// javac Main.java    ← compiles the file
// java Main          ← runs the compiled class
```

**Why bytecode?** Bytecode is platform-independent. The JVM on any OS can execute the same `.class` file.

---

### 1.5 Variables

**Definition:** A variable is a **named container** that stores a value in memory. You must declare a variable with a type before using it.

```java
public class VariableDemo {
    public static void main(String[] args) {

        // Declaring and initializing variables
        int age = 25;              // integer variable
        double salary = 50000.50;  // decimal variable
        char grade = 'A';         // single character
        boolean isActive = true;  // true or false
        String name = "Dharmendra"; // text (reference type)

        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Salary: " + salary);
        System.out.println("Grade: " + grade);
        System.out.println("Active: " + isActive);
    }
}
```

**Rules for naming variables:**
- Must start with a letter, `_` or `$`
- Cannot start with a digit
- Cannot use reserved keywords (like `int`, `class`)
- Java is case-sensitive (`age` and `Age` are different)

---

### 1.6 Data Types

**Definition:** Data types define what kind of value a variable can hold and how much memory it occupies.

#### Primitive Data Types (8 types)

| Type | Size | Range | Example |
|------|------|-------|---------|
| `byte` | 1 byte | -128 to 127 | `byte b = 100;` |
| `short` | 2 bytes | -32,768 to 32,767 | `short s = 1000;` |
| `int` | 4 bytes | -2.1 billion to 2.1 billion | `int num = 50000;` |
| `long` | 8 bytes | Very large numbers | `long l = 100000L;` |
| `float` | 4 bytes | Decimal (6-7 digits precision) | `float f = 3.14f;` |
| `double` | 8 bytes | Decimal (15 digits precision) | `double d = 3.14159;` |
| `char` | 2 bytes | Single character (Unicode) | `char c = 'A';` |
| `boolean` | 1 bit | `true` or `false` | `boolean flag = true;` |

#### Reference Data Types
These hold references (addresses) to objects, not the actual value.
```java
String name = "Java";          // String object
int[] numbers = {1, 2, 3};    // Array object
```

---

### 1.7 Type Casting

**Definition:** Type casting is converting a value from one data type to another.

#### Widening (Implicit) – Automatic, small to big
```java
int myInt = 100;
long myLong = myInt;      // int → long (automatic)
double myDouble = myLong; // long → double (automatic)
System.out.println(myDouble); // 100.0
```

#### Narrowing (Explicit) – Manual, big to small
```java
double myDouble = 9.78;
int myInt = (int) myDouble;  // double → int (manual cast)
System.out.println(myInt);   // 9 (decimal part is lost)
```

**Remember the order:** `byte → short → int → long → float → double` (widening is automatic in this direction).

---

### 1.8 Operators

**Definition:** Operators are special symbols that perform operations on variables and values.

```java
public class OperatorDemo {
    public static void main(String[] args) {

        // 1. Arithmetic Operators
        int a = 10, b = 3;
        System.out.println("Add: " + (a + b));   // 13
        System.out.println("Sub: " + (a - b));   // 7
        System.out.println("Mul: " + (a * b));   // 30
        System.out.println("Div: " + (a / b));   // 3 (integer division)
        System.out.println("Mod: " + (a % b));   // 1 (remainder)

        // 2. Relational (Comparison) Operators
        System.out.println(a > b);   // true
        System.out.println(a == b);  // false
        System.out.println(a != b);  // true

        // 3. Logical Operators
        boolean x = true, y = false;
        System.out.println(x && y);  // false (AND – both must be true)
        System.out.println(x || y);  // true  (OR – at least one true)
        System.out.println(!x);      // false (NOT – reverses the value)

        // 4. Assignment Operators
        int c = 10;
        c += 5;  // c = c + 5 → 15
        c -= 3;  // c = c - 3 → 12
        c *= 2;  // c = c * 2 → 24

        // 5. Unary Operators
        int d = 5;
        d++;  // d becomes 6 (post-increment)
        ++d;  // d becomes 7 (pre-increment)
        d--;  // d becomes 6 (post-decrement)

        // 6. Ternary Operator (shorthand if-else)
        int age = 20;
        String result = (age >= 18) ? "Adult" : "Minor";
        System.out.println(result); // Adult
    }
}
```

---

### 1.9 User Input (Scanner)

**Definition:** The `Scanner` class (from `java.util` package) is used to take input from the user through the keyboard/console.

```java
import java.util.Scanner;  // import the Scanner class

public class InputDemo {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);  // create Scanner object

        // Reading different types of input
        System.out.print("Enter your name: ");
        String name = sc.nextLine();  // reads a full line of text

        System.out.print("Enter your age: ");
        int age = sc.nextInt();  // reads an integer

        System.out.print("Enter your salary: ");
        double salary = sc.nextDouble();  // reads a decimal number

        System.out.println("\n--- Your Details ---");
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Salary: " + salary);

        sc.close();  // always close the scanner when done
    }
}
```

**Common Scanner methods:**
| Method | What it reads |
|--------|--------------|
| `nextLine()` | Full line of text |
| `next()` | Single word |
| `nextInt()` | Integer |
| `nextDouble()` | Decimal number |
| `nextBoolean()` | true/false |

---

### 1.10 Comments

**Definition:** Comments are notes in the code that are **ignored by the compiler**. They help explain the code to other developers (or your future self).

```java
// This is a single-line comment

/*
   This is a multi-line comment.
   It can span across many lines.
*/

/**
 * This is a Javadoc comment.
 * It is used to generate documentation.
 * @param args command-line arguments
 */
public static void main(String[] args) {
    System.out.println("Comments are helpful!");
}
```

---

### 1.11 Keywords

**Definition:** Keywords are **reserved words** in Java that have a predefined meaning. You **cannot** use them as variable names, class names, or identifiers.

**Some important keywords:**
`abstract`, `boolean`, `break`, `byte`, `case`, `catch`, `char`, `class`, `continue`, `default`, `do`, `double`, `else`, `extends`, `final`, `finally`, `float`, `for`, `if`, `implements`, `import`, `int`, `interface`, `long`, `new`, `package`, `private`, `protected`, `public`, `return`, `short`, `static`, `super`, `switch`, `synchronized`, `this`, `throw`, `throws`, `try`, `void`, `while`

**Note:** `true`, `false`, and `null` are literals, not keywords, but they are also reserved.

---

### 1.12 Identifiers

**Definition:** Identifiers are **names given by the programmer** to classes, methods, variables, packages, etc.

**Rules:**
1. Can contain letters (A-Z, a-z), digits (0-9), underscore `_`, and dollar sign `$`.
2. Must **not** start with a digit.
3. Must **not** be a keyword.
4. Java is **case-sensitive** (`myVar` ≠ `MyVar`).

```java
// Valid identifiers
int studentAge = 20;
String _name = "Java";
double $price = 99.99;
int myVar2 = 10;

// Invalid identifiers
// int 2ndNumber = 5;   ← starts with digit
// int class = 10;      ← keyword
// int my-var = 5;      ← hyphen not allowed
```

---

## 2. Control Statements

**Definition:** Control statements determine the **flow of execution** of a program. They let you make decisions, repeat actions, and skip/break out of loops.

---

### 2.1 if Statement

**Definition:** Executes a block of code **only if** the condition is `true`.

```java
int age = 20;

if (age >= 18) {
    System.out.println("You are an adult.");  // this runs because 20 >= 18
}
```

---

### 2.2 if-else Statement

**Definition:** Executes one block if the condition is `true`, and a **different block** if it is `false`.

```java
int marks = 35;

if (marks >= 40) {
    System.out.println("Pass ✅");
} else {
    System.out.println("Fail ❌");  // this runs because 35 < 40
}
```

---

### 2.3 Nested if

**Definition:** An `if` statement **inside** another `if` statement. Used when you need to check multiple conditions in a hierarchy.

```java
int age = 25;
boolean hasID = true;

if (age >= 18) {
    if (hasID) {
        System.out.println("Entry allowed ✅");  // both conditions are true
    } else {
        System.out.println("Bring your ID card!");
    }
} else {
    System.out.println("You are too young.");
}
```

---

### 2.4 Switch Statement

**Definition:** A cleaner alternative to multiple `if-else` blocks. It checks a variable against **multiple possible values** (cases).

```java
int day = 3;

switch (day) {
    case 1:
        System.out.println("Monday");
        break;  // exit the switch block
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");  // this runs
        break;
    case 4:
        System.out.println("Thursday");
        break;
    case 5:
        System.out.println("Friday");
        break;
    default:
        System.out.println("Weekend!");  // runs if no case matches
}
```

**Important:** Always use `break` after each case, otherwise execution will **fall through** to the next case.

---

### 2.5 for Loop

**Definition:** Repeats a block of code a **known number of times**. It has three parts: initialization, condition, and update.

```java
// Print numbers 1 to 5
for (int i = 1; i <= 5; i++) {
    System.out.println("Number: " + i);
}
// Output: 1, 2, 3, 4, 5
```

**How it works:**
1. `int i = 1` → initialize (runs once)
2. `i <= 5` → check condition (before each iteration)
3. `i++` → update (after each iteration)

---

### 2.6 while Loop

**Definition:** Repeats a block of code **as long as** the condition is `true`. The condition is checked **before** each iteration.

```java
int count = 1;

while (count <= 5) {
    System.out.println("Count: " + count);
    count++;  // important! otherwise infinite loop
}
```

**When to use:** When you don't know in advance how many times the loop should run.

---

### 2.7 do-while Loop

**Definition:** Similar to `while`, but the condition is checked **after** the block executes. This means the code runs **at least once**, even if the condition is false.

```java
int num = 10;

do {
    System.out.println("Number: " + num);  // runs at least once
    num++;
} while (num <= 5);  // condition is false, so loop stops after one run
```

---

### 2.8 Enhanced for Loop (for-each)

**Definition:** A simplified `for` loop designed for iterating over **arrays** and **collections**. You don't need an index variable.

```java
String[] fruits = {"Apple", "Banana", "Mango", "Orange"};

// Enhanced for loop – reads each element one by one
for (String fruit : fruits) {
    System.out.println(fruit);
}
// Output: Apple, Banana, Mango, Orange
```

**Syntax:** `for (Type variable : array/collection) { … }`

---

### 2.9 break Statement

**Definition:** Immediately **exits** the current loop or switch block.

```java
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;  // exit loop when i is 5
    }
    System.out.println(i);
}
// Output: 1, 2, 3, 4  (5 is never printed, loop stops)
```

---

### 2.10 continue Statement

**Definition:** **Skips** the current iteration and jumps to the next iteration of the loop.

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue;  // skip when i is 3
    }
    System.out.println(i);
}
// Output: 1, 2, 4, 5  (3 is skipped)
```

---

## 3. Methods

**Definition:** A method is a **reusable block of code** that performs a specific task. Methods help organize code, avoid repetition, and make programs easier to read and maintain.

---

### 3.1 Method Declaration

**Definition:** Declaring a method means defining its structure – the access modifier, return type, name, and parameters.

```java
//  accessModifier  returnType  methodName(parameters)
    public static   int         add(int a, int b) {
        return a + b;  // return the result
    }
```

---

### 3.2 Method Calling

**Definition:** Calling (or invoking) a method means **executing** the code inside it.

```java
public class MethodDemo {

    // Method declaration
    public static void sayHello() {
        System.out.println("Hello, World!");
    }

    public static void main(String[] args) {
        sayHello();  // Method call – executes the code inside sayHello
        sayHello();  // You can call it multiple times
    }
}
```

---

### 3.3 Parameters

**Definition:** Parameters are **inputs** passed to a method so it can work with different values each time it is called.

```java
public class MethodDemo {

    // Method with parameters
    public static void greet(String name, int age) {
        System.out.println("Hello " + name + ", you are " + age + " years old.");
    }

    public static void main(String[] args) {
        greet("Dharmendra", 25);  // passing arguments
        greet("Rahul", 30);       // different arguments
    }
}
```

---

### 3.4 Return Type

**Definition:** The return type specifies what kind of value a method **sends back** to the caller. If a method returns nothing, we use `void`.

```java
public class MethodDemo {

    // Method that returns a value
    public static int multiply(int a, int b) {
        return a * b;  // sends the result back
    }

    // Method that returns nothing
    public static void printMessage() {
        System.out.println("This method returns void.");
    }

    public static void main(String[] args) {
        int result = multiply(4, 5);  // capture the returned value
        System.out.println("Result: " + result);  // 20

        printMessage();  // no return value to capture
    }
}
```

---

### 3.5 Method Overloading

**Definition:** Having **multiple methods with the same name** but **different parameter lists** (different number or types of parameters). The compiler picks the correct method based on the arguments.

```java
public class Calculator {

    // Two parameters
    public static int add(int a, int b) {
        return a + b;
    }

    // Three parameters (overloaded)
    public static int add(int a, int b, int c) {
        return a + b + c;
    }

    // Different type (overloaded)
    public static double add(double a, double b) {
        return a + b;
    }

    public static void main(String[] args) {
        System.out.println(add(2, 3));        // calls add(int, int) → 5
        System.out.println(add(1, 2, 3));     // calls add(int, int, int) → 6
        System.out.println(add(2.5, 3.5));    // calls add(double, double) → 6.0
    }
}
```

---

### 3.6 Variable Arguments (Varargs)

**Definition:** Varargs allow a method to accept **any number of arguments** of the same type. Internally, Java treats them as an array.

**Syntax:** `type... variableName`

```java
public class VarargsDemo {

    // Method accepts any number of integers
    public static int sum(int... numbers) {
        int total = 0;
        for (int n : numbers) {
            total += n;
        }
        return total;
    }

    public static void main(String[] args) {
        System.out.println(sum(1, 2));           // 3
        System.out.println(sum(1, 2, 3, 4, 5));  // 15
        System.out.println(sum(10));              // 10
        System.out.println(sum());                // 0 (no arguments)
    }
}
```

**Rule:** Varargs must be the **last parameter** in the method signature.

---

### 3.7 Recursion

**Definition:** Recursion is when a method **calls itself** to solve a smaller version of the same problem. Every recursive method must have a **base case** (stopping condition) to avoid infinite recursion.

```java
public class RecursionDemo {

    // Factorial: 5! = 5 × 4 × 3 × 2 × 1 = 120
    public static long factorial(int n) {
        if (n <= 1) {
            return 1;  // base case – stops the recursion
        }
        return n * factorial(n - 1);  // recursive call
    }

    public static void main(String[] args) {
        System.out.println("5! = " + factorial(5));  // 120
        System.out.println("0! = " + factorial(0));  // 1
    }
}
```

**How it works step by step:**
```
factorial(5) → 5 * factorial(4)
             → 5 * 4 * factorial(3)
             → 5 * 4 * 3 * factorial(2)
             → 5 * 4 * 3 * 2 * factorial(1)
             → 5 * 4 * 3 * 2 * 1
             = 120
```

---

## 4. Arrays

**Definition:** An array is a **fixed-size container** that stores multiple values of the **same data type** in contiguous memory. Each element is accessed using an **index** (starting from 0).

---

### 4.1 1D Array

```java
// Declaration and initialization
int[] numbers = {10, 20, 30, 40, 50};

// Accessing elements by index
System.out.println(numbers[0]);  // 10 (first element)
System.out.println(numbers[4]);  // 50 (last element)

// Changing an element
numbers[2] = 99;
System.out.println(numbers[2]);  // 99

// Length of array
System.out.println("Size: " + numbers.length);  // 5
```

---

### 4.2 2D Array

**Definition:** A 2D array is an **array of arrays** – like a table with rows and columns (matrix).

```java
// 2D array (3 rows × 3 columns)
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Access element at row 1, column 2
System.out.println(matrix[1][2]);  // 6

// Print entire matrix
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
// Output:
// 1 2 3
// 4 5 6
// 7 8 9
```

---

### 4.3 Array Operations

```java
import java.util.Arrays;

int[] nums = {5, 3, 1, 4, 2};

// Fill all elements with a value
int[] filled = new int[5];
Arrays.fill(filled, 7);  // [7, 7, 7, 7, 7]

// Convert array to string for printing
System.out.println(Arrays.toString(nums));  // [5, 3, 1, 4, 2]

// Check equality
int[] a = {1, 2, 3};
int[] b = {1, 2, 3};
System.out.println(Arrays.equals(a, b));  // true
```

---

### 4.4 Array Traversal

```java
int[] nums = {10, 20, 30, 40, 50};

// Method 1: for loop
for (int i = 0; i < nums.length; i++) {
    System.out.println("Index " + i + " → " + nums[i]);
}

// Method 2: enhanced for loop (for-each)
for (int n : nums) {
    System.out.println(n);
}
```

---

### 4.5 Copy Arrays

```java
int[] original = {1, 2, 3, 4, 5};

// Method 1: System.arraycopy
int[] copy1 = new int[5];
System.arraycopy(original, 0, copy1, 0, original.length);

// Method 2: Arrays.copyOf
int[] copy2 = Arrays.copyOf(original, original.length);

// Method 3: clone
int[] copy3 = original.clone();
```

---

### 4.6 Sorting Arrays

```java
import java.util.Arrays;

int[] nums = {5, 1, 4, 2, 3};
Arrays.sort(nums);  // sorts in ascending order
System.out.println(Arrays.toString(nums));  // [1, 2, 3, 4, 5]
```

---

### 4.7 Searching Arrays

```java
import java.util.Arrays;

int[] nums = {1, 2, 3, 4, 5};  // must be sorted for binarySearch

int index = Arrays.binarySearch(nums, 3);
System.out.println("Found 3 at index: " + index);  // 2

int notFound = Arrays.binarySearch(nums, 9);
System.out.println("9 not found, result: " + notFound);  // negative value
```

---

## 5. Strings

---

### 5.1 String

**Definition:** A `String` is a sequence of characters. In Java, strings are **objects** of the `String` class and are **immutable** (cannot be changed after creation).

```java
// Creating strings
String s1 = "Hello";              // string literal (stored in String Pool)
String s2 = new String("Hello");  // using new keyword (stored in heap)

System.out.println(s1);          // Hello
System.out.println(s1.length()); // 5
```

---

### 5.2 String Pool

**Definition:** The String Pool is a special area in Java memory (inside the heap) where **string literals** are stored. If two variables have the same literal value, Java reuses the same object to save memory.

```java
String a = "Java";
String b = "Java";
String c = new String("Java");

System.out.println(a == b);       // true  (same object in pool)
System.out.println(a == c);       // false (different objects)
System.out.println(a.equals(c));  // true  (same content)
```

**Rule:** Always use `.equals()` to compare string **content**, not `==`.

---

### 5.3 String vs StringBuilder vs StringBuffer

| Feature | String | StringBuilder | StringBuffer |
|---------|--------|---------------|--------------|
| **Mutability** | Immutable ❌ | Mutable ✅ | Mutable ✅ |
| **Thread-safe** | Yes (immutable) | No ❌ | Yes ✅ (synchronized) |
| **Performance** | Slow for many changes | Fast ⚡ | Slower than StringBuilder |
| **Use when** | Fixed text | Many changes, single thread | Many changes, multi-threaded |

```java
// String – creates new object on every change
String s = "Hello";
s = s + " World";  // new String object created

// StringBuilder – modifies the same object (fast)
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World");  // same object modified
System.out.println(sb.toString());  // Hello World

// StringBuffer – like StringBuilder but thread-safe
StringBuffer sbf = new StringBuffer("Hello");
sbf.append(" World");
System.out.println(sbf.toString());  // Hello World
```

---

### 5.4 String Methods

```java
String s = "Hello, Java World!";

s.length();                    // 18 – number of characters
s.charAt(0);                   // 'H' – character at index 0
s.substring(7, 11);            // "Java" – extract part of string
s.toUpperCase();               // "HELLO, JAVA WORLD!"
s.toLowerCase();               // "hello, java world!"
s.trim();                      // removes leading/trailing spaces
s.contains("Java");            // true – checks if substring exists
s.equals("Hello");             // false – compares content
s.equalsIgnoreCase("hello, java world!");  // true
s.indexOf("Java");             // 7 – position of substring
s.replace("Java", "Python");   // "Hello, Python World!"
s.startsWith("Hello");         // true
s.endsWith("!");               // true
s.isEmpty();                   // false
s.split(", ");                 // ["Hello", "Java World!"]
```

---

### 5.5 Immutable Strings

**Definition:** Once a String object is created, its value **cannot be changed**. Any modification creates a **new** String object in memory.

```java
String original = "Hello";
String modified = original.concat(" World");

System.out.println(original);  // "Hello" – original is unchanged!
System.out.println(modified);  // "Hello World" – new object created
```

**Why immutable?**
- **Security** – Strings used in passwords, URLs, file paths cannot be altered accidentally.
- **Thread safety** – Multiple threads can share strings safely.
- **Performance** – Enables the String Pool for memory optimization.

---

# 📘 Phase 2: Object-Oriented Programming (Must-Know)

---

## 6. Classes & Objects

---

### 6.1 Class

**Definition:** A class is a **blueprint** or template that defines the **properties** (fields) and **behaviors** (methods) of objects. It does not occupy memory until an object is created.

```java
// Class = Blueprint
public class Car {
    // Fields (properties)
    String brand;
    String color;
    int speed;

    // Method (behavior)
    public void drive() {
        System.out.println(brand + " is driving at " + speed + " km/h.");
    }
}
```

---

### 6.2 Object

**Definition:** An object is an **instance** of a class. It is a real entity created from the class blueprint that occupies memory.

```java
public class Main {
    public static void main(String[] args) {
        // Creating objects from the Car class
        Car car1 = new Car();       // object 1
        car1.brand = "Toyota";
        car1.color = "Red";
        car1.speed = 120;
        car1.drive();  // Toyota is driving at 120 km/h.

        Car car2 = new Car();       // object 2
        car2.brand = "BMW";
        car2.color = "Black";
        car2.speed = 200;
        car2.drive();  // BMW is driving at 200 km/h.
    }
}
```

---

### 6.3 Instance Variables

**Definition:** Variables declared **inside a class but outside any method**. Each object gets its **own copy** of instance variables.

```java
public class Student {
    String name;   // instance variable – unique per object
    int rollNo;    // instance variable
}
```

---

### 6.4 Local Variables

**Definition:** Variables declared **inside a method**. They exist only during the method's execution and are **destroyed** when the method ends.

```java
public void calculateSum() {
    int a = 10;  // local variable
    int b = 20;  // local variable
    int sum = a + b;
    System.out.println("Sum: " + sum);
}
// a, b, sum do NOT exist outside this method
```

---

### 6.5 Static Variables

**Definition:** Variables declared with the `static` keyword belong to the **class itself**, not to any particular object. All objects **share** the same copy.

```java
public class Counter {
    static int count = 0;  // shared by all objects

    public Counter() {
        count++;  // incremented for every new object
    }

    public static void main(String[] args) {
        new Counter();
        new Counter();
        new Counter();
        System.out.println("Objects created: " + Counter.count);  // 3
    }
}
```

---

### 6.6 Constructors

**Definition:** A constructor is a **special method** that runs automatically when an object is created. It has the **same name as the class** and **no return type**. Used to initialize object fields.

```java
public class Person {
    String name;
    int age;

    // Default constructor
    public Person() {
        name = "Unknown";
        age = 0;
    }

    // Parameterized constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void display() {
        System.out.println(name + " - " + age);
    }

    public static void main(String[] args) {
        Person p1 = new Person();                // calls default constructor
        Person p2 = new Person("Dharmendra", 25); // calls parameterized constructor

        p1.display();  // Unknown - 0
        p2.display();  // Dharmendra - 25
    }
}
```

---

### 6.7 this Keyword

**Definition:** `this` refers to the **current object** inside a class. It is mainly used to differentiate between instance variables and parameters when they have the same name.

```java
public class Employee {
    String name;
    double salary;

    public Employee(String name, double salary) {
        this.name = name;      // this.name = instance variable, name = parameter
        this.salary = salary;
    }

    public void show() {
        System.out.println(this.name + " earns " + this.salary);
    }
}
```

---

## 7. OOP Concepts

---

### 7.1 Encapsulation

**Definition:** Encapsulation means **wrapping data (fields) and methods together** inside a class and **restricting direct access** to the fields by making them `private`. Access is given through `public` getter and setter methods.

**Real-life analogy:** An ATM machine 🏧 – you interact with buttons (methods), but you can't directly access the cash vault (private data).

```java
public class BankAccount {
    private double balance;  // private – cannot be accessed directly

    // Getter – read the balance
    public double getBalance() {
        return balance;
    }

    // Setter – modify the balance with validation
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        } else {
            System.out.println("Invalid amount!");
        }
    }

    public static void main(String[] args) {
        BankAccount acc = new BankAccount();
        acc.deposit(5000);
        System.out.println("Balance: " + acc.getBalance());  // 5000.0
        // acc.balance = -100;  ← Compile error! Cannot access private field
    }
}
```

---

### 7.2 Inheritance

**Definition:** Inheritance allows a class (**child/subclass**) to **inherit** the properties and methods of another class (**parent/superclass**). It promotes **code reuse**.

**Real-life analogy:** A child inherits traits from parents 👨‍👩‍👦 – eye color, height, etc.

```java
// Parent class
class Animal {
    String name;

    public void eat() {
        System.out.println(name + " is eating.");
    }

    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
}

// Child class – inherits from Animal
class Dog extends Animal {

    public void bark() {
        System.out.println(name + " says Woof! 🐕");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.name = "Buddy";
        dog.eat();    // inherited from Animal
        dog.sleep();  // inherited from Animal
        dog.bark();   // own method
    }
}
```

**Types of inheritance in Java:**
- **Single** – One child, one parent.
- **Multilevel** – A → B → C (chain).
- **Hierarchical** – One parent, multiple children.
- **Note:** Java does **NOT** support **multiple inheritance** with classes (but supports it with interfaces).

---

### 7.3 Polymorphism

**Definition:** Polymorphism means **"many forms"**. The same method name can behave differently depending on the context. Java supports two types:

#### Compile-time Polymorphism (Method Overloading)
Same method name, different parameter lists – resolved at **compile time**.

```java
class Printer {
    void print(String text) {
        System.out.println("Text: " + text);
    }

    void print(int number) {
        System.out.println("Number: " + number);
    }

    void print(String text, int times) {
        for (int i = 0; i < times; i++) {
            System.out.println(text);
        }
    }
}
```

#### Runtime Polymorphism (Method Overriding)
Child class **overrides** a parent's method – resolved at **runtime**.

```java
class Shape {
    void draw() {
        System.out.println("Drawing a shape.");
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

public class Main {
    public static void main(String[] args) {
        Shape s1 = new Circle();  // parent reference, child object
        Shape s2 = new Square();
        s1.draw();  // Drawing a circle ⭕  (runtime decision)
        s2.draw();  // Drawing a square 🟥
    }
}
```

---

### 7.4 Method Overloading (detail)

**Definition:** Same method name with **different parameters** (number, type, or order). It is a form of **compile-time polymorphism**.

**Rules:**
- Method name must be the **same**.
- Parameters must be **different**.
- Return type alone is **not enough** to overload.

---

### 7.5 Method Overriding (detail)

**Definition:** A child class provides its **own implementation** of a method that is already defined in the parent class. The method signature must be **exactly the same**.

**Rules:**
- Method name, return type, and parameters must be **identical**.
- Use `@Override` annotation (optional but recommended).
- Access modifier in child must be **same or more permissive**.
- `static`, `final`, and `private` methods **cannot** be overridden.

---

### 7.6 Abstraction

**Definition:** Abstraction means **hiding complex implementation details** and showing only the essential features. It is achieved using **abstract classes** and **interfaces**.

```java
// Abstract class – cannot be instantiated
abstract class Vehicle {
    abstract void start();  // abstract method – no body

    void stop() {  // concrete method – has a body
        System.out.println("Vehicle stopped.");
    }
}

// Concrete class – must implement all abstract methods
class Car extends Vehicle {
    @Override
    void start() {
        System.out.println("Car started with key 🔑");
    }
}

public class Main {
    public static void main(String[] args) {
        // Vehicle v = new Vehicle();  ← Error! Cannot instantiate abstract class
        Vehicle car = new Car();
        car.start();  // Car started with key 🔑
        car.stop();   // Vehicle stopped.
    }
}
```

---

### 7.7 Interface

**Definition:** An interface is a **fully abstract contract**. It defines method signatures without implementation. A class **implements** an interface and provides the actual code. Java supports **multiple interface implementation**.

```java
// Interface
interface Drivable {
    void drive();   // abstract by default
    void brake();
}

interface Playable {
    void playMusic();
}

// Class implements multiple interfaces
class SmartCar implements Drivable, Playable {
    @Override
    public void drive() { System.out.println("Driving..."); }

    @Override
    public void brake() { System.out.println("Braking..."); }

    @Override
    public void playMusic() { System.out.println("Playing music 🎵"); }
}
```

---

## 8. Keywords (Detailed)

---

### 8.1 this
Refers to the **current object**. Used to access instance variables, call constructors, or pass the current object as an argument.

### 8.2 super
Refers to the **parent class**. Used to call parent's constructor, access parent's methods, or access parent's fields that are hidden by the child.

```java
class Animal {
    String type = "Animal";
    void sound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    String type = "Dog";

    void display() {
        System.out.println(type);         // Dog
        System.out.println(super.type);   // Animal (parent's field)
        super.sound();                    // calls parent's method
    }
}
```

### 8.3 final
- **final variable** – value cannot be changed (constant).
- **final method** – cannot be overridden by child classes.
- **final class** – cannot be extended (no subclasses).

```java
final int MAX = 100;       // constant
// MAX = 200;              ← Error!

final class MathUtils { }  // cannot extend this class
```

### 8.4 static
Belongs to the **class** rather than any object instance. Can be accessed without creating an object.

```java
class MathHelper {
    static int add(int a, int b) { return a + b; }
}
// Call without object:
MathHelper.add(3, 5);
```

### 8.5 abstract
- **abstract class** – cannot be instantiated; may have abstract methods.
- **abstract method** – declared without a body; must be implemented by subclasses.

### 8.6 synchronized
Ensures only **one thread at a time** can execute a method or block – prevents race conditions.

### 8.7 volatile
Ensures a variable is always **read from and written to main memory**, not from a thread's local cache. Used in multithreading.

### 8.8 transient
Marks a field to be **excluded from serialization** – its value won't be saved when the object is serialized.

---

## 9. Access Modifiers

**Definition:** Access modifiers control the **visibility** (who can access) of classes, methods, and variables.

| Modifier | Same Class | Same Package | Subclass (other package) | Everywhere |
|----------|:----------:|:------------:|:------------------------:|:----------:|
| `public` | ✅ | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| *default* (no modifier) | ✅ | ✅ | ❌ | ❌ |
| `private` | ✅ | ❌ | ❌ | ❌ |

```java
public class Example {
    public int a = 1;       // accessible everywhere
    protected int b = 2;    // same package + subclasses
    int c = 3;              // default – same package only
    private int d = 4;      // this class only
}
```

---

# 📕 Phase 3: Advanced Core Java

---

## 10. Exception Handling

**Definition:** Exception handling is a mechanism to **handle runtime errors** gracefully so the program doesn't crash unexpectedly. Java uses `try`, `catch`, `finally`, `throw`, and `throws`.

---

### 10.1 try

**Definition:** The `try` block contains code that **might throw an exception**. If an exception occurs, control moves to the `catch` block.

### 10.2 catch

**Definition:** The `catch` block **handles** the exception. It specifies the type of exception to catch.

### 10.3 finally

**Definition:** The `finally` block **always executes**, whether an exception occurs or not. Used for cleanup (closing files, connections, etc.).

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;  // ArithmeticException
            System.out.println(result);  // this line won't execute
        } catch (ArithmeticException e) {
            System.out.println("Error: Cannot divide by zero!");
            System.out.println("Message: " + e.getMessage());
        } finally {
            System.out.println("This always executes (cleanup).");
        }
    }
}
```

### 10.4 throw

**Definition:** `throw` is used to **manually/explicitly** throw an exception.

```java
public static void validateAge(int age) {
    if (age < 18) {
        throw new IllegalArgumentException("Age must be 18 or older!");
    }
    System.out.println("Welcome!");
}
```

### 10.5 throws

**Definition:** `throws` is used in the **method signature** to declare that a method might throw a checked exception. The caller must handle it.

```java
public static void readFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    System.out.println(br.readLine());
    br.close();
}
```

### 10.6 Custom Exception

**Definition:** You can create your own exception class by extending `Exception` (checked) or `RuntimeException` (unchecked).

```java
// Custom checked exception
class InsufficientBalanceException extends Exception {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

class BankAccount {
    private double balance = 1000;

    public void withdraw(double amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException(
                "Cannot withdraw ₹" + amount + ". Balance is only ₹" + balance
            );
        }
        balance -= amount;
        System.out.println("Withdrawn ₹" + amount + ". Remaining: ₹" + balance);
    }
}
```

### 10.7 Checked vs Unchecked Exceptions

| Feature | Checked Exception | Unchecked Exception |
|---------|-------------------|---------------------|
| **When detected** | Compile time | Runtime |
| **Must handle?** | Yes (`try-catch` or `throws`) | No (optional) |
| **Extends** | `Exception` | `RuntimeException` |
| **Examples** | `IOException`, `SQLException` | `NullPointerException`, `ArithmeticException` |

---

## 11. Collections Framework ⭐⭐⭐

**Definition:** The Collections Framework is a set of **interfaces and classes** that provide data structures (like lists, sets, maps) and algorithms (like sorting, searching) for storing and manipulating groups of objects.

---

### 11.1 List – Ordered, allows duplicates

#### ArrayList
**Definition:** A **resizable array** implementation. Fast for **random access** (get by index), slow for insertions/deletions in the middle.

```java
import java.util.ArrayList;
import java.util.List;

List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
names.add("Alice");  // duplicates allowed
System.out.println(names);       // [Alice, Bob, Alice]
System.out.println(names.get(1)); // Bob
names.remove("Alice");           // removes first occurrence
```

#### LinkedList
**Definition:** A **doubly-linked list** implementation. Fast for **insertions/deletions**, slower for random access.

```java
import java.util.LinkedList;

LinkedList<String> list = new LinkedList<>();
list.addFirst("First");
list.addLast("Last");
list.add("Middle");
System.out.println(list);  // [First, Middle, Last]
```

#### Vector
**Definition:** Similar to `ArrayList` but **synchronized** (thread-safe). Slower due to synchronization overhead. Legacy class.

#### Stack
**Definition:** A **LIFO** (Last In, First Out) data structure. `push()` adds to top, `pop()` removes from top.

```java
import java.util.Stack;

Stack<String> stack = new Stack<>();
stack.push("A");
stack.push("B");
stack.push("C");
System.out.println(stack.pop());  // C (last in, first out)
System.out.println(stack.peek()); // B (look at top without removing)
```

---

### 11.2 Set – No duplicates

#### HashSet
**Definition:** Stores unique elements with **no guaranteed order**. Uses hashing internally for **fast lookup** (O(1)).

```java
import java.util.HashSet;
import java.util.Set;

Set<String> cities = new HashSet<>();
cities.add("Delhi");
cities.add("Mumbai");
cities.add("Delhi");  // duplicate – ignored!
System.out.println(cities);  // [Delhi, Mumbai] (order may vary)
```

#### LinkedHashSet
**Definition:** Like `HashSet` but **maintains insertion order**.

#### TreeSet
**Definition:** Stores elements in **sorted (ascending) order**. Uses a Red-Black tree internally.

```java
import java.util.TreeSet;

TreeSet<Integer> ts = new TreeSet<>();
ts.add(30);
ts.add(10);
ts.add(20);
System.out.println(ts);  // [10, 20, 30] (sorted)
```

---

### 11.3 Queue – FIFO order

#### PriorityQueue
**Definition:** Elements are ordered by their **natural ordering** or a custom comparator. The head is always the **smallest** element.

```java
import java.util.PriorityQueue;

PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.add(30);
pq.add(10);
pq.add(20);
System.out.println(pq.poll());  // 10 (smallest first)
System.out.println(pq.poll());  // 20
```

#### Deque & ArrayDeque
**Definition:** A **double-ended queue** – elements can be added or removed from **both ends**. `ArrayDeque` is the most common implementation.

```java
import java.util.ArrayDeque;
import java.util.Deque;

Deque<String> deque = new ArrayDeque<>();
deque.addFirst("Front");
deque.addLast("Back");
System.out.println(deque);  // [Front, Back]
```

---

### 11.4 Map – Key-Value pairs

#### HashMap
**Definition:** Stores data as **key-value pairs**. Keys are unique, values can be duplicated. **No guaranteed order**. Very fast (O(1) for get/put).

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 95);
scores.put("Bob", 88);
scores.put("Alice", 99);  // overwrites previous value for "Alice"
System.out.println(scores);            // {Alice=99, Bob=88}
System.out.println(scores.get("Bob")); // 88
```

#### LinkedHashMap
**Definition:** Like `HashMap` but **maintains insertion order**.

#### TreeMap
**Definition:** Stores entries sorted by **key** in natural order.

#### Hashtable
**Definition:** Legacy synchronized version of `HashMap`. Thread-safe but slower. Does **not** allow `null` keys or values.

#### ConcurrentHashMap
**Definition:** A modern, thread-safe map designed for **high concurrency**. Better performance than `Hashtable` because it locks only parts of the map (segment locking).

---

## 12. Collection Interfaces

**Definition:** Interfaces define the **contract** (what methods are available) without specifying the implementation.

| Interface | Description | Key Methods |
|-----------|-------------|-------------|
| `Collection` | Root interface for all collections (except Map). | `add()`, `remove()`, `size()`, `contains()` |
| `List` | Ordered collection, allows duplicates. | `get(index)`, `set(index, value)`, `indexOf()` |
| `Set` | Collection with **no duplicates**. | `add()`, `contains()`, `remove()` |
| `Queue` | FIFO ordering (First In, First Out). | `offer()`, `poll()`, `peek()` |
| `Map` | Key-value pairs, keys are unique. | `put()`, `get()`, `containsKey()`, `keySet()` |

---

## 13. Generics

**Definition:** Generics allow you to write **type-safe** code that works with any data type. They catch type errors at **compile time** rather than runtime, and eliminate the need for casting.

---

### 13.1 Generic Classes

```java
// A box that can hold any type
class Box<T> {
    private T item;

    public void set(T item) { this.item = item; }
    public T get() { return item; }
}

public class Main {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.set("Hello");
        String value = stringBox.get();  // no casting needed

        Box<Integer> intBox = new Box<>();
        intBox.set(42);
        int num = intBox.get();
    }
}
```

---

### 13.2 Generic Methods

```java
public class Util {
    // Generic method – works with any type
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        Integer[] nums = {1, 2, 3};
        String[] words = {"Hello", "World"};

        printArray(nums);   // 1 2 3
        printArray(words);  // Hello World
    }
}
```

---

### 13.3 Wildcards

**Definition:** Wildcards (`?`) represent an **unknown type** and provide flexibility when working with generics.

```java
import java.util.List;
import java.util.ArrayList;

// Upper bounded wildcard – accepts Number and its subclasses
public static double sum(List<? extends Number> list) {
    double total = 0;
    for (Number n : list) {
        total += n.doubleValue();
    }
    return total;
}

// Lower bounded wildcard – accepts Integer and its superclasses
public static void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
}
```

| Wildcard | Meaning | Use case |
|----------|---------|----------|
| `?` | Any type | Read-only, unknown type |
| `? extends T` | T or its subclasses | Reading (producing) values |
| `? super T` | T or its superclasses | Writing (consuming) values |

---

### 13.4 Type Parameters

Common naming conventions:
- `T` – Type
- `E` – Element (used in collections)
- `K` – Key
- `V` – Value
- `N` – Number

---

## 14. Multithreading

**Definition:** Multithreading is the ability to run **multiple threads (tasks) simultaneously** within a single program. Each thread is a lightweight unit of execution.

---

### 14.1 Thread Class

```java
// Creating a thread by extending Thread class
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 3; i++) {
            System.out.println(Thread.currentThread().getName() + " - Count: " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.setName("Worker-1");
        t1.start();  // starts a new thread (calls run() internally)
    }
}
```

---

### 14.2 Runnable Interface

**Definition:** Another way to create threads – implement the `Runnable` interface. Preferred because Java supports single inheritance only; implementing Runnable keeps the option to extend another class.

```java
class Task implements Runnable {
    @Override
    public void run() {
        System.out.println("Task running on: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(new Task(), "MyThread");
        t.start();
    }
}
```

---

### 14.3 Thread Lifecycle

```
 NEW  →  RUNNABLE  →  RUNNING  →  TERMINATED
             ↕            ↕
         BLOCKED      WAITING / TIMED_WAITING
```

| State | Description |
|-------|-------------|
| **NEW** | Thread object created but `start()` not yet called. |
| **RUNNABLE** | Ready to run, waiting for CPU. |
| **RUNNING** | Actively executing. |
| **BLOCKED** | Waiting to acquire a lock. |
| **WAITING** | Waiting indefinitely for another thread. |
| **TIMED_WAITING** | Waiting for a specified time (`sleep()`, `join(timeout)`). |
| **TERMINATED** | Execution completed. |

---

### 14.4 Synchronization

**Definition:** Synchronization ensures that **only one thread** can access a shared resource at a time, preventing data corruption (race conditions).

```java
class Counter {
    private int count = 0;

    // synchronized method – only one thread can execute this at a time
    public synchronized void increment() {
        count++;
    }

    public int getCount() { return count; }
}
```

---

### 14.5 Deadlock

**Definition:** A situation where **two or more threads are blocked forever**, each waiting for the other to release a lock.

```
Thread-1 holds Lock-A, waiting for Lock-B
Thread-2 holds Lock-B, waiting for Lock-A
→ Both are stuck forever!
```

**How to avoid:** Always acquire locks in the **same order** across all threads.

---

### 14.6 Executor Framework

**Definition:** A higher-level replacement for manually creating threads. It manages a **pool of reusable threads**.

```java
import java.util.concurrent.*;

public class ExecutorDemo {
    public static void main(String[] args) {
        ExecutorService pool = Executors.newFixedThreadPool(3);

        for (int i = 1; i <= 5; i++) {
            final int taskId = i;
            pool.submit(() -> {
                System.out.println("Task " + taskId + " on " + Thread.currentThread().getName());
            });
        }

        pool.shutdown();  // no new tasks accepted; existing tasks finish
    }
}
```

---

### 14.7 Callable & 14.8 Future

**Definition:** `Callable` is like `Runnable` but it can **return a value** and **throw exceptions**. `Future` holds the result of an asynchronous computation.

```java
import java.util.concurrent.*;

public class CallableDemo {
    public static void main(String[] args) throws Exception {
        ExecutorService pool = Executors.newSingleThreadExecutor();

        // Callable – returns a result
        Future<Integer> future = pool.submit(() -> {
            Thread.sleep(1000);
            return 42;
        });

        System.out.println("Doing other work...");
        Integer result = future.get();  // blocks until result is ready
        System.out.println("Result: " + result);  // 42

        pool.shutdown();
    }
}
```

---

### 14.9 Thread Pool

**Definition:** A thread pool is a collection of **pre-created, reusable threads**. Instead of creating a new thread for every task, tasks are submitted to the pool and executed by available threads.

| Pool Type | Method | Description |
|-----------|--------|-------------|
| Fixed | `Executors.newFixedThreadPool(n)` | Exactly `n` threads |
| Cached | `Executors.newCachedThreadPool()` | Creates threads as needed, reuses idle ones |
| Single | `Executors.newSingleThreadExecutor()` | Only 1 thread |
| Scheduled | `Executors.newScheduledThreadPool(n)` | For delayed/periodic tasks |

---

## 15. File Handling

**Definition:** File handling allows you to **read from and write to files** on the disk using Java I/O classes.

---

### 15.1 File Class

```java
import java.io.File;

File file = new File("test.txt");
System.out.println("Exists: " + file.exists());
System.out.println("Name: " + file.getName());
System.out.println("Path: " + file.getAbsolutePath());
System.out.println("Size: " + file.length() + " bytes");
```

---

### 15.2 FileReader & 15.3 FileWriter

```java
import java.io.*;

// FileWriter – writes characters to a file
FileWriter fw = new FileWriter("output.txt");
fw.write("Hello, File!");
fw.close();

// FileReader – reads characters from a file
FileReader fr = new FileReader("output.txt");
int ch;
while ((ch = fr.read()) != -1) {
    System.out.print((char) ch);
}
fr.close();
```

---

### 15.4 BufferedReader & 15.5 BufferedWriter

**Definition:** Buffered readers/writers use an internal **buffer** to read/write data in chunks, making I/O much **faster**.

```java
import java.io.*;

// BufferedWriter – efficient writing
try (BufferedWriter bw = new BufferedWriter(new FileWriter("notes.txt"))) {
    bw.write("Line 1: Hello");
    bw.newLine();  // adds a new line
    bw.write("Line 2: World");
}

// BufferedReader – efficient reading
try (BufferedReader br = new BufferedReader(new FileReader("notes.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
}
```

---

### 15.6 Serialization & 15.7 Deserialization

**Serialization:** Converting an object into a **byte stream** so it can be saved to a file or sent over a network.

**Deserialization:** Reconstructing the object from the byte stream.

```java
import java.io.*;

// The class must implement Serializable
class Student implements Serializable {
    private static final long serialVersionUID = 1L;
    String name;
    int age;
    transient String password;  // transient – will NOT be serialized

    Student(String name, int age, String password) {
        this.name = name;
        this.age = age;
        this.password = password;
    }
}

public class SerializationDemo {
    public static void main(String[] args) throws Exception {

        Student s = new Student("Dharmendra", 25, "secret123");

        // Serialize – write object to file
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.ser"))) {
            oos.writeObject(s);
            System.out.println("Object serialized ✅");
        }

        // Deserialize – read object from file
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.ser"))) {
            Student loaded = (Student) ois.readObject();
            System.out.println("Name: " + loaded.name);       // Dharmendra
            System.out.println("Age: " + loaded.age);          // 25
            System.out.println("Password: " + loaded.password); // null (transient)
        }
    }
}
```

---

## 16. Wrapper Classes

**Definition:** Wrapper classes convert **primitive types into objects**. They are needed because collections (like `ArrayList`) can only store objects, not primitives.

| Primitive | Wrapper Class |
|-----------|--------------|
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

### Autoboxing & Unboxing

```java
// Autoboxing – primitive → wrapper (automatic)
Integer num = 10;  // int 10 is automatically wrapped into Integer object

// Unboxing – wrapper → primitive (automatic)
int value = num;   // Integer object is automatically unwrapped to int

// Useful methods
int parsed = Integer.parseInt("123");        // String → int
String str = Integer.toString(123);          // int → String
int max = Integer.max(10, 20);               // 20
```

---

## 17. Enums

**Definition:** An `enum` (enumeration) is a special class that represents a **fixed set of constants**. Enums are type-safe, meaning you can't assign any random value.

```java
public enum Season {
    SPRING, SUMMER, AUTUMN, WINTER
}

public class EnumDemo {
    public static void main(String[] args) {
        Season current = Season.SUMMER;
        System.out.println("Current season: " + current);  // SUMMER

        // Loop through all enum values
        for (Season s : Season.values()) {
            System.out.println(s);
        }

        // Switch with enum
        switch (current) {
            case SUMMER: System.out.println("It's hot! ☀️"); break;
            case WINTER: System.out.println("It's cold! ❄️"); break;
            default: System.out.println("Nice weather!");
        }
    }
}
```

**Enum with fields and methods:**
```java
public enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    EARTH(5.976e+24, 6.37814e6),
    MARS(6.421e+23, 3.3972e6);

    private final double mass;
    private final double radius;

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }

    public double getMass() { return mass; }
    public double getRadius() { return radius; }
}
```

---

## 18. Annotations

**Definition:** Annotations are **metadata** (data about data) that provide additional information to the compiler, runtime, or tools. They do not directly affect the program logic.

---

### 18.1 @Override

**Definition:** Tells the compiler that the method is meant to **override** a method in the parent class. If the method doesn't actually override anything, the compiler throws an error.

```java
class Animal {
    void sound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    @Override  // compiler verifies this method exists in parent
    void sound() { System.out.println("Woof!"); }
}
```

---

### 18.2 @Deprecated

**Definition:** Marks a method, class, or field as **outdated**. The IDE shows a warning when someone uses it.

```java
class OldApi {
    @Deprecated
    public void oldMethod() {
        System.out.println("This method is old. Use newMethod() instead.");
    }

    public void newMethod() {
        System.out.println("This is the new way.");
    }
}
```

---

### 18.3 @SuppressWarnings

**Definition:** Tells the compiler to **suppress** (hide) specific warnings.

```java
@SuppressWarnings("unchecked")   // suppress unchecked cast warnings
List raw = new ArrayList();       // raw type – normally shows warning

@SuppressWarnings("deprecation") // suppress deprecation warnings
OldApi api = new OldApi();
api.oldMethod();                  // no warning shown
```

---

### 18.4 Custom Annotations

**Definition:** You can create your **own annotations** to attach metadata to classes, methods, or fields.

```java
import java.lang.annotation.*;

// Define custom annotation
@Retention(RetentionPolicy.RUNTIME)  // available at runtime via reflection
@Target(ElementType.METHOD)          // can only be applied to methods
public @interface RunTest {
    String value() default "default test"; // optional attribute with default
    int priority() default 1;
}

// Using the custom annotation
class TestSuite {

    @RunTest(value = "Login Test", priority = 1)
    public void testLogin() {
        System.out.println("Testing login...");
    }

    @RunTest(value = "Signup Test", priority = 2)
    public void testSignup() {
        System.out.println("Testing signup...");
    }
}
```

**Retention Policies:**
| Policy | Meaning |
|--------|---------|
| `SOURCE` | Discarded by the compiler (not in bytecode). |
| `CLASS` | Stored in bytecode but not available at runtime. |
| `RUNTIME` | Available at runtime via **reflection**. |

---

# ✅ Summary Checklist

After completing these notes, you should be able to:

- [ ] Write and compile a Java program
- [ ] Use all data types, operators, and control statements
- [ ] Create methods with overloading, varargs, and recursion
- [ ] Work with arrays and strings effectively
- [ ] Design classes with OOP principles (encapsulation, inheritance, polymorphism, abstraction)
- [ ] Handle exceptions gracefully
- [ ] Use the Collections Framework (List, Set, Map, Queue)
- [ ] Write generic classes and methods
- [ ] Create and manage threads
- [ ] Read and write files
- [ ] Understand wrapper classes, enums, and annotations

---

**Happy Coding! 🎉**
