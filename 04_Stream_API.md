# 4. Stream API

> The Stream API is the **most powerful feature** of Java 8. It lets you process collections of data in a clean, readable, and efficient way.

---

## 🧠 Theory – Understanding Streams

### What is a Stream in Simple Words?
A Stream is like a **conveyor belt in a factory**. Raw materials (data) go in on one side, they pass through different machines (operations like filter, map, sort), and the finished product comes out the other side.

> ⚠️ Important: A Stream is NOT a data structure. It does NOT store data. It just **processes** data from a source (like a List or Array).

### Real-Life Analogy – Water Pipeline 🚰
Think of a **water purification system**:

```
River Water → [Filter Mud] → [Remove Chemicals] → [Add Minerals] → Clean Water 🥤
```

- **Source** = River (your List or Array)
- **Filter Mud** = `filter()` – remove unwanted elements
- **Remove Chemicals** = `map()` – transform elements
- **Add Minerals** = another `map()` – add/change something
- **Clean Water** = `collect()` – final result

The water keeps flowing through each stage. The river doesn't change – only the output is different!

### Real-Life Analogy – Amazon Order Processing 📦
When you order from **Amazon**, your order goes through a pipeline:

```
All Orders → [Filter: My City] → [Sort: By Priority] → [Map: Add Tracking] → [Collect: Deliver] 📬
```

| Pipeline Stage | Stream Operation | What It Does |
|---------------|-----------------|-------------|
| Warehouse has all items | `list.stream()` | Start the stream |
| Pick only Delhi orders | `.filter(order -> order.city.equals("Delhi"))` | Keep matching items |
| Sort by delivery date | `.sorted(byDate)` | Arrange in order |
| Add tracking number | `.map(order -> addTracking(order))` | Transform each item |
| Load delivery trucks | `.collect(toList())` | Gather final results |

### Two Types of Operations

Think of it like a **cooking recipe**:
- **Intermediate operations** (preparation steps) = Chop onions, wash vegetables, marinate chicken → These steps **prepare** but don't give you the final dish. You can chain as many as you want.
- **Terminal operations** (final step) = Put in oven / Serve on plate → This **produces the result**. Once done, cooking is over.

> 🔑 **Key Rule**: Nothing happens until a terminal operation is called! Just like chopping vegetables without ever cooking them – no meal is produced.

### Lazy Evaluation – Why Streams are Smart 🧠
Streams are **lazy** – they don't do any work until the terminal operation is called. This is like a **lazy student** who doesn't start the assignment until the deadline (terminal operation) arrives.

**Why is this good?** Because the stream can **optimize** the work:
```java
// Stream finds FIRST match and STOPS – doesn't process entire list!
String result = hugeList.stream()
    .filter(s -> s.startsWith("A"))
    .findFirst()    // terminal – triggers work, stops after first match
    .orElse("None");
```

---

## 4.1 What is a Stream?

A **Stream** is a sequence of elements that supports **pipeline operations** like filter, map, and reduce.

> Think of it like a conveyor belt in a factory:
> **Data goes in → Operations happen → Result comes out**

### Key Points
- A stream **does NOT store** data – it processes data from a source (like a List).
- A stream **does NOT modify** the original collection.
- Operations are **lazy** – they don't run until a terminal operation is called.
- A stream can only be **used once**.

---

## 4.2 How to Create a Stream

```java
// From a List
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Stream<String> stream1 = names.stream();

// From an Array
String[] arr = {"A", "B", "C"};
Stream<String> stream2 = Arrays.stream(arr);

// Using Stream.of()
Stream<Integer> stream3 = Stream.of(1, 2, 3, 4, 5);

// Using Stream.generate() – infinite stream
Stream<Double> randoms = Stream.generate(Math::random).limit(5);

// Using Stream.iterate() – infinite stream with pattern
Stream<Integer> evens = Stream.iterate(0, n -> n + 2).limit(10);
// 0, 2, 4, 6, 8, 10, 12, 14, 16, 18

// From a String (characters)
IntStream chars = "Hello".chars();
```

---

## 4.3 Stream Operations Overview

There are two types of operations:

| Type | What It Does | Returns | Examples |
|------|-------------|---------|---------|
| **Intermediate** | Transforms the stream | Another Stream | `filter`, `map`, `sorted`, `distinct`, `limit`, `skip`, `flatMap`, `peek` |
| **Terminal** | Produces a result or side-effect | A value or void | `forEach`, `collect`, `count`, `reduce`, `min`, `max`, `anyMatch`, `allMatch`, `findFirst` |

> ⚠️ **Nothing happens until a terminal operation is called!** (Lazy evaluation)

---

## 4.4 filter() – Keep elements that match a condition

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Keep only even numbers
List<Integer> evens = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

System.out.println(evens); // [2, 4, 6, 8, 10]
```

### Filter Strings
```java
List<String> names = Arrays.asList("Alice", "Bob", "Anna", "Adam", "Charlie");

// Names starting with 'A'
List<String> aNames = names.stream()
    .filter(name -> name.startsWith("A"))
    .collect(Collectors.toList());

System.out.println(aNames); // [Alice, Anna, Adam]
```

---

## 4.5 map() – Transform each element

```java
List<String> names = Arrays.asList("alice", "bob", "charlie");

// Convert to uppercase
List<String> upperNames = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

System.out.println(upperNames); // [ALICE, BOB, CHARLIE]
```

### Map to different type
```java
List<String> words = Arrays.asList("Java", "is", "awesome");

// Get the length of each word
List<Integer> lengths = words.stream()
    .map(String::length)
    .collect(Collectors.toList());

System.out.println(lengths); // [4, 2, 7]
```

---

## 4.6 sorted() – Sort elements

```java
List<String> names = Arrays.asList("Charlie", "Alice", "Bob");

// Natural order (A-Z)
List<String> sorted = names.stream()
    .sorted()
    .collect(Collectors.toList());
System.out.println(sorted); // [Alice, Bob, Charlie]

// Reverse order (Z-A)
List<String> reversed = names.stream()
    .sorted(Comparator.reverseOrder())
    .collect(Collectors.toList());
System.out.println(reversed); // [Charlie, Bob, Alice]

// Sort by length
List<String> byLength = names.stream()
    .sorted(Comparator.comparingInt(String::length))
    .collect(Collectors.toList());
System.out.println(byLength); // [Bob, Alice, Charlie]
```

---

## 4.7 distinct(), limit(), skip()

```java
List<Integer> nums = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 5);

// Remove duplicates
nums.stream().distinct().forEach(System.out::print); // 1 2 3 4 5

System.out.println();

// Take first 3
nums.stream().limit(3).forEach(System.out::print); // 1 2 2

System.out.println();

// Skip first 5
nums.stream().skip(5).forEach(System.out::print); // 3 4 5
```

---

## 4.8 flatMap() – Flatten nested collections

```java
List<List<String>> nested = Arrays.asList(
    Arrays.asList("a", "b"),
    Arrays.asList("c", "d"),
    Arrays.asList("e", "f")
);

// Without flatMap → Stream<List<String>>
// With flatMap   → Stream<String>
List<String> flat = nested.stream()
    .flatMap(Collection::stream)
    .collect(Collectors.toList());

System.out.println(flat); // [a, b, c, d, e, f]
```

### Real-world Example – Split sentences into words
```java
List<String> sentences = Arrays.asList(
    "Java is great",
    "Streams are powerful"
);

List<String> words = sentences.stream()
    .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
    .collect(Collectors.toList());

System.out.println(words); // [Java, is, great, Streams, are, powerful]
```

---

## 4.9 reduce() – Combine all elements into one value

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Sum all numbers
int sum = numbers.stream()
    .reduce(0, (a, b) -> a + b);
System.out.println("Sum: " + sum); // Sum: 15

// Same with method reference
int sum2 = numbers.stream()
    .reduce(0, Integer::sum);

// Find maximum
Optional<Integer> max = numbers.stream()
    .reduce(Integer::max);
max.ifPresent(m -> System.out.println("Max: " + m)); // Max: 5

// Concatenate strings
List<String> words = Arrays.asList("Java", "8", "is", "cool");
String sentence = words.stream()
    .reduce("", (a, b) -> a + " " + b)
    .trim();
System.out.println(sentence); // Java 8 is cool
```

---

## 4.10 Terminal Operations

### forEach – Perform action on each element
```java
names.stream().forEach(System.out::println);
```

### count – Count elements
```java
long count = names.stream()
    .filter(n -> n.length() > 3)
    .count();
System.out.println(count);
```

### min / max
```java
Optional<Integer> min = numbers.stream().min(Integer::compareTo);
Optional<Integer> max = numbers.stream().max(Integer::compareTo);
```

### anyMatch / allMatch / noneMatch
```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);

boolean hasEven = nums.stream().anyMatch(n -> n % 2 == 0);   // true
boolean allEven = nums.stream().allMatch(n -> n % 2 == 0);   // false
boolean noneNeg = nums.stream().noneMatch(n -> n < 0);       // true
```

### findFirst / findAny
```java
Optional<String> first = names.stream()
    .filter(n -> n.startsWith("A"))
    .findFirst();

first.ifPresent(System.out::println); // Alice
```

---

## 4.11 Primitive Streams – IntStream, LongStream, DoubleStream

These avoid **autoboxing** overhead.

```java
// Create
IntStream ints = IntStream.rangeClosed(1, 5); // 1, 2, 3, 4, 5

// Sum
int total = IntStream.rangeClosed(1, 100).sum();
System.out.println(total); // 5050

// Average
OptionalDouble avg = IntStream.of(10, 20, 30).average();
avg.ifPresent(System.out::println); // 20.0

// Convert List<Integer> to IntStream
List<Integer> nums = Arrays.asList(1, 2, 3);
int sum = nums.stream()
    .mapToInt(Integer::intValue)
    .sum();
```

---

## 4.12 Parallel Streams

Run stream operations on **multiple CPU cores** for large datasets.

```java
List<Integer> bigList = IntStream.rangeClosed(1, 1_000_000)
    .boxed()
    .collect(Collectors.toList());

// Sequential
long start1 = System.currentTimeMillis();
long count1 = bigList.stream()
    .filter(n -> n % 2 == 0)
    .count();
long time1 = System.currentTimeMillis() - start1;

// Parallel
long start2 = System.currentTimeMillis();
long count2 = bigList.parallelStream()
    .filter(n -> n % 2 == 0)
    .count();
long time2 = System.currentTimeMillis() - start2;

System.out.println("Sequential: " + time1 + "ms");
System.out.println("Parallel:   " + time2 + "ms");
```

### ⚠️ When to use Parallel Streams
| ✅ Use When | ❌ Avoid When |
|-------------|-------------|
| Very large collections (100K+) | Small collections |
| CPU-bound operations | I/O-bound operations |
| Order doesn't matter | Order matters |
| No shared mutable state | Modifying shared variables |

---

## 4.13 Complete Example – Employee Processing

```java
class Employee {
    String name;
    String department;
    double salary;

    Employee(String name, String department, double salary) {
        this.name = name;
        this.department = department;
        this.salary = salary;
    }
    // getters...
}

List<Employee> employees = Arrays.asList(
    new Employee("Alice",   "IT",    75000),
    new Employee("Bob",     "HR",    60000),
    new Employee("Charlie", "IT",    80000),
    new Employee("Diana",   "HR",    65000),
    new Employee("Eve",     "IT",    90000)
);

// 1. Names of IT employees earning > 70K, sorted
List<String> result = employees.stream()
    .filter(e -> e.department.equals("IT"))
    .filter(e -> e.salary > 70000)
    .sorted(Comparator.comparingDouble(e -> e.salary))
    .map(e -> e.name)
    .collect(Collectors.toList());

System.out.println(result); // [Alice, Charlie, Eve]

// 2. Total salary of all employees
double totalSalary = employees.stream()
    .mapToDouble(e -> e.salary)
    .sum();
System.out.println("Total: " + totalSalary); // 370000.0

// 3. Highest paid employee
Optional<Employee> highest = employees.stream()
    .max(Comparator.comparingDouble(e -> e.salary));
highest.ifPresent(e -> System.out.println("Highest: " + e.name)); // Eve
```

---

## ✅ Stream Operations Cheat Sheet

```
Source → .filter() → .map() → .sorted() → .collect()
                                            .forEach()
                                            .count()
                                            .reduce()
```

| Operation | Type | What It Does |
|-----------|------|-------------|
| `filter(Predicate)` | Intermediate | Keep matching elements |
| `map(Function)` | Intermediate | Transform elements |
| `flatMap(Function)` | Intermediate | Flatten nested streams |
| `sorted()` | Intermediate | Sort elements |
| `distinct()` | Intermediate | Remove duplicates |
| `limit(n)` | Intermediate | Take first n |
| `skip(n)` | Intermediate | Skip first n |
| `peek(Consumer)` | Intermediate | Debug / inspect |
| `forEach(Consumer)` | Terminal | Do something with each |
| `collect(Collector)` | Terminal | Gather into collection |
| `count()` | Terminal | Count elements |
| `reduce(BinaryOperator)` | Terminal | Combine into one value |
| `min() / max()` | Terminal | Find smallest / largest |
| `anyMatch / allMatch / noneMatch` | Terminal | Test conditions |
| `findFirst / findAny` | Terminal | Get one element |

---

## ✅ Checklist

- [ ] I can create streams from lists, arrays, and generators
- [ ] I can use filter, map, sorted, distinct, limit, skip
- [ ] I understand flatMap and can flatten nested lists
- [ ] I can use reduce to combine elements
- [ ] I know the difference between intermediate and terminal operations
- [ ] I understand when to use parallel streams

**Next → [05_Optional.md](./05_Optional.md)**
