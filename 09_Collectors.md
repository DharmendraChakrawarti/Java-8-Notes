# 9. Collectors & Grouping

> `Collectors` is a utility class that provides ready-made implementations for `collect()` – the most common terminal operation in streams.

---

## 🧠 Theory – Understanding Collectors

### What are Collectors in Simple Words?
After a stream processes your data (filter, map, sort), you need to **gather the results** somewhere. Collectors decide **how** and **where** the results are collected.

Think of it like a **factory assembly line**:
- The conveyor belt (stream) processes items
- At the END of the belt, a **worker (collector)** picks up the finished items and puts them in:
  - A **box** → `toList()` (ordered collection)
  - A **bag** → `toSet()` (no duplicates)
  - A **labeled shelf** → `toMap()` (key-value pairs)
  - **Groups of boxes** → `groupingBy()` (categorized)

### Real-Life Analogy – School Report Card 🏫
Imagine a school principal wants reports on students:

| What Principal Wants | Collector to Use | Code |
|---------------------|-----------------|------|
| "List all student names" | `toList()` | `.collect(Collectors.toList())` |
| "List unique subjects" | `toSet()` | `.collect(Collectors.toSet())` |
| "Student name → marks mapping" | `toMap()` | `.collect(Collectors.toMap(name, marks))` |
| "Group students by class" | `groupingBy()` | `.collect(Collectors.groupingBy(Student::getClass))` |
| "Pass vs Fail students" | `partitioningBy()` | `.collect(Collectors.partitioningBy(s -> s.marks >= 40))` |
| "Average marks of all students" | `averagingInt()` | `.collect(Collectors.averagingInt(Student::getMarks))` |
| "Comma-separated names for certificate" | `joining()` | `.collect(Collectors.joining(", "))` |
| "How many students?" | `counting()` | `.collect(Collectors.counting())` |

### Real-Life Analogy – Census Department 📊
The **Census of India** collects data about citizens:

```
All Citizens (Stream Source)
  → Group by State (groupingBy)
  → Count per State (counting)
  → Average Income per State (averagingDouble)
  → Partition: Urban vs Rural (partitioningBy)
```

This is exactly how `Collectors.groupingBy()` works with **downstream collectors**!

### groupingBy vs partitioningBy
| Feature | `groupingBy` | `partitioningBy` |
|---------|-------------|-----------------|
| Number of groups | **Any number** (by key) | **Exactly 2** (true/false) |
| Key type | Any type (String, Integer, etc.) | Always `Boolean` |
| Real-life example | Group students by class (A, B, C, D) | Split students into Pass/Fail |
| Returns | `Map<K, List<T>>` | `Map<Boolean, List<T>>` |

---

## 9.1 What is collect()?

`collect()` is a terminal operation that gathers stream elements into a **collection** or a **single result**.

```java
// Basic pattern
List<String> result = stream.collect(Collectors.toList());
```

---

## 9.2 Collecting to Collections

### toList()
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

List<String> filtered = names.stream()
    .filter(n -> n.length() > 3)
    .collect(Collectors.toList());

System.out.println(filtered); // [Alice, Charlie]
```

### toSet() – No duplicates
```java
List<Integer> nums = Arrays.asList(1, 2, 2, 3, 3, 3);

Set<Integer> unique = nums.stream()
    .collect(Collectors.toSet());

System.out.println(unique); // [1, 2, 3]
```

### toMap()
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Name → Length
Map<String, Integer> nameLength = names.stream()
    .collect(Collectors.toMap(
        name -> name,           // key
        name -> name.length()   // value
    ));

System.out.println(nameLength);
// {Alice=5, Bob=3, Charlie=7}
```

### Handling duplicate keys in toMap()
```java
List<String> words = Arrays.asList("hello", "world", "hello");

// ❌ This will throw IllegalStateException (duplicate key "hello")
// Map<String, Integer> map = words.stream()
//     .collect(Collectors.toMap(w -> w, w -> 1));

// ✅ Use merge function for duplicates
Map<String, Integer> wordCount = words.stream()
    .collect(Collectors.toMap(
        w -> w,            // key
        w -> 1,            // value
        Integer::sum       // merge function (add counts)
    ));

System.out.println(wordCount); // {hello=2, world=1}
```

### toCollection() – Specific collection type
```java
TreeSet<String> sorted = names.stream()
    .collect(Collectors.toCollection(TreeSet::new));

System.out.println(sorted); // [Alice, Bob, Charlie] (sorted)
```

---

## 9.3 Joining Strings

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Simple join
String joined = names.stream()
    .collect(Collectors.joining());
System.out.println(joined); // AliceBobCharlie

// Join with delimiter
String csv = names.stream()
    .collect(Collectors.joining(", "));
System.out.println(csv); // Alice, Bob, Charlie

// Join with delimiter, prefix, suffix
String formatted = names.stream()
    .collect(Collectors.joining(", ", "[", "]"));
System.out.println(formatted); // [Alice, Bob, Charlie]
```

---

## 9.4 Counting

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Diana");

long count = names.stream()
    .filter(n -> n.length() > 3)
    .collect(Collectors.counting());

System.out.println(count); // 3 (Alice, Charlie, Diana)
```

---

## 9.5 Summarizing (Statistics)

```java
List<Integer> numbers = Arrays.asList(10, 20, 30, 40, 50);

// Get all stats at once
IntSummaryStatistics stats = numbers.stream()
    .collect(Collectors.summarizingInt(Integer::intValue));

System.out.println("Count:   " + stats.getCount());    // 5
System.out.println("Sum:     " + stats.getSum());      // 150
System.out.println("Min:     " + stats.getMin());      // 10
System.out.println("Max:     " + stats.getMax());      // 50
System.out.println("Average: " + stats.getAverage());  // 30.0
```

### Individual operations
```java
// Sum
int sum = numbers.stream()
    .collect(Collectors.summingInt(Integer::intValue));

// Average
Double avg = numbers.stream()
    .collect(Collectors.averagingInt(Integer::intValue));

// Max
Optional<Integer> max = numbers.stream()
    .collect(Collectors.maxBy(Integer::compareTo));

// Min
Optional<Integer> min = numbers.stream()
    .collect(Collectors.minBy(Integer::compareTo));
```

---

## 9.6 groupingBy() – Group elements

This is one of the **most powerful** collectors!

### Simple grouping
```java
class Employee {
    String name;
    String department;
    double salary;

    Employee(String name, String dept, double salary) {
        this.name = name;
        this.department = dept;
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

// Group by department
Map<String, List<Employee>> byDept = employees.stream()
    .collect(Collectors.groupingBy(e -> e.department));

// Result:
// IT → [Alice, Charlie, Eve]
// HR → [Bob, Diana]
```

### Group and count
```java
Map<String, Long> countByDept = employees.stream()
    .collect(Collectors.groupingBy(
        e -> e.department,
        Collectors.counting()
    ));

System.out.println(countByDept); // {IT=3, HR=2}
```

### Group and sum
```java
Map<String, Double> salaryByDept = employees.stream()
    .collect(Collectors.groupingBy(
        e -> e.department,
        Collectors.summingDouble(e -> e.salary)
    ));

System.out.println(salaryByDept); // {IT=245000.0, HR=125000.0}
```

### Group and get average
```java
Map<String, Double> avgSalary = employees.stream()
    .collect(Collectors.groupingBy(
        e -> e.department,
        Collectors.averagingDouble(e -> e.salary)
    ));

System.out.println(avgSalary); // {IT=81666.67, HR=62500.0}
```

### Group and get names only
```java
Map<String, List<String>> namesByDept = employees.stream()
    .collect(Collectors.groupingBy(
        e -> e.department,
        Collectors.mapping(e -> e.name, Collectors.toList())
    ));

System.out.println(namesByDept); // {IT=[Alice, Charlie, Eve], HR=[Bob, Diana]}
```

### Group and get max salary per department
```java
Map<String, Optional<Employee>> highestPaid = employees.stream()
    .collect(Collectors.groupingBy(
        e -> e.department,
        Collectors.maxBy(Comparator.comparingDouble(e -> e.salary))
    ));
```

### Multi-level grouping
```java
// Group by department, then by salary range
Map<String, Map<String, List<Employee>>> multiLevel = employees.stream()
    .collect(Collectors.groupingBy(
        e -> e.department,
        Collectors.groupingBy(
            e -> e.salary > 70000 ? "HIGH" : "LOW"
        )
    ));
```

---

## 9.7 partitioningBy() – Split into two groups

Returns a `Map<Boolean, List<T>>` – elements split by true/false.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

Map<Boolean, List<Integer>> evenOdd = numbers.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));

System.out.println("Even: " + evenOdd.get(true));   // [2, 4, 6, 8, 10]
System.out.println("Odd:  " + evenOdd.get(false));  // [1, 3, 5, 7, 9]
```

### Partition with downstream collector
```java
Map<Boolean, Long> evenOddCount = numbers.stream()
    .collect(Collectors.partitioningBy(
        n -> n % 2 == 0,
        Collectors.counting()
    ));

System.out.println(evenOddCount); // {false=5, true=5}
```

---

## 9.8 collectingAndThen() – Collect then transform

```java
// Collect to list, then make it unmodifiable
List<String> immutableList = names.stream()
    .collect(Collectors.collectingAndThen(
        Collectors.toList(),
        Collections::unmodifiableList
    ));

// immutableList.add("New"); // ❌ UnsupportedOperationException
```

---

## 9.9 Complete Example – Student Report

```java
class Student {
    String name;
    String grade;  // A, B, C
    int marks;

    Student(String name, String grade, int marks) {
        this.name = name;
        this.grade = grade;
        this.marks = marks;
    }
}

List<Student> students = Arrays.asList(
    new Student("Alice",   "A", 95),
    new Student("Bob",     "B", 78),
    new Student("Charlie", "A", 88),
    new Student("Diana",   "C", 65),
    new Student("Eve",     "B", 82),
    new Student("Frank",   "A", 91)
);

// 1. Group by grade
Map<String, List<String>> namesByGrade = students.stream()
    .collect(Collectors.groupingBy(
        s -> s.grade,
        Collectors.mapping(s -> s.name, Collectors.toList())
    ));
System.out.println(namesByGrade);
// {A=[Alice, Charlie, Frank], B=[Bob, Eve], C=[Diana]}

// 2. Average marks by grade
Map<String, Double> avgByGrade = students.stream()
    .collect(Collectors.groupingBy(
        s -> s.grade,
        Collectors.averagingInt(s -> s.marks)
    ));
System.out.println(avgByGrade);
// {A=91.33, B=80.0, C=65.0}

// 3. Comma-separated names
String allNames = students.stream()
    .map(s -> s.name)
    .collect(Collectors.joining(", "));
System.out.println(allNames);
// Alice, Bob, Charlie, Diana, Eve, Frank

// 4. Pass/Fail partition (pass = marks >= 70)
Map<Boolean, List<String>> passFail = students.stream()
    .collect(Collectors.partitioningBy(
        s -> s.marks >= 70,
        Collectors.mapping(s -> s.name, Collectors.toList())
    ));
System.out.println("Pass: " + passFail.get(true));   // [Alice, Bob, Charlie, Eve, Frank]
System.out.println("Fail: " + passFail.get(false));  // [Diana]
```

---

## ✅ Collectors Cheat Sheet

| Collector | Result | Use Case |
|-----------|--------|----------|
| `toList()` | `List<T>` | Most common |
| `toSet()` | `Set<T>` | Remove duplicates |
| `toMap(keyFn, valueFn)` | `Map<K,V>` | Key-value pairs |
| `joining(", ")` | `String` | Concatenate strings |
| `counting()` | `Long` | Count elements |
| `summingInt(fn)` | `Integer` | Sum values |
| `averagingInt(fn)` | `Double` | Average values |
| `maxBy(comparator)` | `Optional<T>` | Maximum element |
| `minBy(comparator)` | `Optional<T>` | Minimum element |
| `groupingBy(fn)` | `Map<K, List<T>>` | Group elements |
| `partitioningBy(pred)` | `Map<Boolean, List<T>>` | Split true/false |
| `summarizingInt(fn)` | `IntSummaryStatistics` | All stats at once |
| `collectingAndThen(c, fn)` | Varies | Collect then transform |
| `mapping(fn, downstream)` | Varies | Map inside grouping |

---

## ✅ Checklist

- [ ] I can collect to List, Set, and Map
- [ ] I can join strings with delimiters
- [ ] I can group elements by a property
- [ ] I can use downstream collectors (counting, summing, averaging)
- [ ] I understand partitioningBy vs groupingBy
- [ ] I can do multi-level grouping

**Next → [10_CompletableFuture.md](./10_CompletableFuture.md)**
