# 11. Java 8 Interview Questions & Answers

> 50+ most asked Java 8 interview questions with easy-to-understand answers and code examples. Organized by topic – study the ones relevant to your interview level.

---

## 🟢 Section A – Lambda & Functional Interfaces (Beginner)

---

### Q1. What are the main features of Java 8?
**Answer:**
1. **Lambda Expressions** – Short anonymous functions
2. **Stream API** – Process collections in pipeline style
3. **Optional** – Avoid NullPointerException
4. **Functional Interfaces** – Predicate, Function, Consumer, Supplier
5. **Default & Static methods in Interfaces**
6. **Method References** – Shorthand for lambdas
7. **New Date-Time API** – `java.time` package
8. **CompletableFuture** – Async programming
9. **Nashorn JavaScript Engine**

---

### Q2. What is a Lambda Expression? Give an example.
**Answer:**
A lambda is a short way to write a function without a name. It implements a functional interface.

```java
// Syntax: (parameters) -> { body }

// Example 1: No parameters
Runnable r = () -> System.out.println("Hello!");

// Example 2: One parameter
Consumer<String> print = s -> System.out.println(s);

// Example 3: Two parameters
Comparator<Integer> compare = (a, b) -> a - b;
```

---

### Q3. What is a Functional Interface?
**Answer:**
An interface with **exactly one abstract method**. It can have multiple default/static methods.

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);  // only ONE abstract method
    
    // These are allowed:
    default void print() { System.out.println("Calculator"); }
    static void info() { System.out.println("v1.0"); }
}

// Usage with lambda
Calculator add = (a, b) -> a + b;
System.out.println(add.calculate(5, 3)); // 8
```

---

### Q4. Name the 4 main built-in functional interfaces.
**Answer:**

| Interface | Method | Takes | Returns | Example |
|-----------|--------|-------|---------|---------|
| `Predicate<T>` | `test(T)` | T | boolean | `n -> n > 0` |
| `Function<T,R>` | `apply(T)` | T | R | `s -> s.length()` |
| `Consumer<T>` | `accept(T)` | T | void | `s -> System.out.println(s)` |
| `Supplier<T>` | `get()` | nothing | T | `() -> new ArrayList<>()` |

---

### Q5. What is the difference between Predicate and Function?
**Answer:**
- **Predicate** → always returns `boolean` (true/false). Used for filtering.
- **Function** → returns any type. Used for transforming.

```java
Predicate<String> isLong = s -> s.length() > 5;    // returns boolean
Function<String, Integer> length = s -> s.length(); // returns Integer
```

---

### Q6. What are Method References? Give examples of each type.
**Answer:**
Method references are shorthand for lambdas that just call one existing method.

```java
// 1. Static method reference
Function<String, Integer> parse = Integer::parseInt;

// 2. Instance method of specific object
Consumer<String> print = System.out::println;

// 3. Instance method of arbitrary object
Function<String, String> upper = String::toUpperCase;

// 4. Constructor reference
Supplier<List<String>> newList = ArrayList::new;
```

---

### Q7. Can a lambda modify a local variable?
**Answer:**
**No.** Variables used inside a lambda must be **effectively final** (not modified after initialization).

```java
int x = 10;
// x = 20;  // ❌ If you uncomment this, lambda below will NOT compile

Runnable r = () -> System.out.println(x);  // x must be effectively final
```

---

## 🟡 Section B – Stream API (Intermediate)

---

### Q8. What is a Stream in Java 8?
**Answer:**
A Stream is a **sequence of elements** that supports operations like filter, map, reduce to process data in a pipeline. It does NOT store data and does NOT modify the source.

```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
int sum = nums.stream()
    .filter(n -> n % 2 == 0)  // keep even
    .mapToInt(n -> n)          // convert
    .sum();                    // add up
System.out.println(sum); // 6
```

---

### Q9. What is the difference between Intermediate and Terminal operations?
**Answer:**

| Intermediate | Terminal |
|-------------|----------|
| Returns another Stream | Returns a result or void |
| Lazy (don't execute until terminal is called) | Triggers execution |
| `filter`, `map`, `sorted`, `distinct` | `forEach`, `collect`, `count`, `reduce` |

---

### Q10. Difference between `map()` and `flatMap()`?
**Answer:**
- `map()` → transforms each element (1-to-1)
- `flatMap()` → transforms and flattens nested structures (1-to-many)

```java
// map → each element becomes one element
List<String> words = Arrays.asList("hello", "world");
List<Integer> lengths = words.stream()
    .map(String::length)
    .collect(Collectors.toList());
// [5, 5]

// flatMap → each element becomes multiple elements
List<List<Integer>> nested = Arrays.asList(
    Arrays.asList(1, 2),
    Arrays.asList(3, 4)
);
List<Integer> flat = nested.stream()
    .flatMap(Collection::stream)
    .collect(Collectors.toList());
// [1, 2, 3, 4]
```

---

### Q11. How does `reduce()` work?
**Answer:**
`reduce()` combines all elements into a single value using a binary operation.

```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);

// With identity (starting value)
int sum = nums.stream().reduce(0, Integer::sum);  // 15

// Without identity (returns Optional)
Optional<Integer> max = nums.stream().reduce(Integer::max);  // Optional[5]
```

---

### Q12. What is the difference between `Collection.stream()` and `Collection.parallelStream()`?
**Answer:**
- `stream()` → processes elements **one by one** (single thread)
- `parallelStream()` → processes elements **in parallel** (multiple threads/cores)

```java
// Use parallel for large datasets with CPU-heavy operations
long count = bigList.parallelStream()
    .filter(n -> isPrime(n))
    .count();
```

> ⚠️ Don't use parallel for small lists, I/O operations, or when order matters.

---

### Q13. Find the second highest number in a list using streams.
**Answer:**
```java
List<Integer> nums = Arrays.asList(5, 3, 9, 1, 7, 9, 2);

Optional<Integer> secondHighest = nums.stream()
    .distinct()
    .sorted(Comparator.reverseOrder())
    .skip(1)
    .findFirst();

System.out.println(secondHighest.get()); // 7
```

---

### Q14. Count the occurrence of each character in a string using streams.
**Answer:**
```java
String input = "programming";

Map<Character, Long> charCount = input.chars()
    .mapToObj(c -> (char) c)
    .collect(Collectors.groupingBy(c -> c, Collectors.counting()));

System.out.println(charCount);
// {p=1, r=2, o=1, g=2, a=1, m=2, i=1, n=1}
```

---

### Q15. Find duplicate elements in a list using streams.
**Answer:**
```java
List<Integer> nums = Arrays.asList(1, 2, 3, 2, 4, 3, 5);

Set<Integer> seen = new HashSet<>();
List<Integer> duplicates = nums.stream()
    .filter(n -> !seen.add(n))  // add returns false if already present
    .collect(Collectors.toList());

System.out.println(duplicates); // [2, 3]
```

---

## 🟡 Section C – Optional (Intermediate)

---

### Q16. What is Optional and why was it introduced?
**Answer:**
`Optional` is a container that may or may not hold a value. It was introduced to **avoid NullPointerException** and make null-handling explicit.

```java
// Without Optional – risky
String name = user.getName(); // might be null → NPE! 💥

// With Optional – safe
Optional<String> name = Optional.ofNullable(user.getName());
String result = name.orElse("Unknown");
```

---

### Q17. Difference between `Optional.of()` and `Optional.ofNullable()`?
**Answer:**
- `Optional.of(value)` → value **must NOT be null** (throws NPE if null)
- `Optional.ofNullable(value)` → value **can be null** (returns empty Optional)

```java
Optional<String> a = Optional.of("Hello");      // ✅ OK
// Optional<String> b = Optional.of(null);       // ❌ NullPointerException!
Optional<String> c = Optional.ofNullable(null);  // ✅ OK → empty Optional
```

---

### Q18. Difference between `orElse()` and `orElseGet()`?
**Answer:**
- `orElse(value)` → **always evaluates** the default value
- `orElseGet(supplier)` → **evaluates only if empty** (lazy)

```java
// orElse – getDefault() is called even if Optional has value
String r1 = optional.orElse(getDefault());

// orElseGet – getDefault() called ONLY if optional is empty
String r2 = optional.orElseGet(() -> getDefault());

// Use orElseGet when default is expensive to create
```

---

### Q19. How to use `map()` and `flatMap()` with Optional?
**Answer:**
```java
Optional<String> name = Optional.of("dharmendra");

// map – transform the value
Optional<String> upper = name.map(String::toUpperCase); // DHARMENDRA

// flatMap – when the function itself returns Optional
Optional<String> email = name.flatMap(n -> getEmail(n)); // avoids Optional<Optional<>>
```

---

## 🔴 Section D – Default Methods & Interfaces (Intermediate)

---

### Q20. Why were default methods added to interfaces?
**Answer:**
To **add new methods to existing interfaces without breaking** all the classes that implement them. For example, `forEach()` was added to `Iterable` as a default method.

```java
interface Vehicle {
    void start();
    default void honk() { System.out.println("Beep!"); }  // free for all implementations
}
```

---

### Q21. What happens if a class implements two interfaces with the same default method?
**Answer:**
**Compilation error!** You must **override** the method and choose which one to call.

```java
interface A { default void hello() { System.out.println("A"); } }
interface B { default void hello() { System.out.println("B"); } }

class C implements A, B {
    @Override
    public void hello() {
        A.super.hello();  // choose A's version
    }
}
```

---

### Q22. Can interfaces have static methods in Java 8?
**Answer:**
**Yes!** Static methods belong to the interface itself and are called using the interface name.

```java
interface MathUtils {
    static int square(int n) { return n * n; }
}

int result = MathUtils.square(5); // 25
```

---

## 🔴 Section E – Date-Time API (Intermediate)

---

### Q23. Why was the new Date-Time API introduced?
**Answer:**
The old `Date` and `Calendar` classes were:
- **Mutable** (not thread-safe)
- **Confusing** (months start from 0)
- **Poorly designed** (formatting not thread-safe)

The new `java.time` API is immutable, thread-safe, and easy to use.

---

### Q24. What is the difference between LocalDate, LocalTime, and LocalDateTime?
**Answer:**

| Class | Contains | Example |
|-------|----------|---------|
| `LocalDate` | Date only | `2026-07-18` |
| `LocalTime` | Time only | `14:30:00` |
| `LocalDateTime` | Both | `2026-07-18T14:30:00` |

```java
LocalDate date = LocalDate.now();         // 2026-07-18
LocalTime time = LocalTime.now();         // 14:30:45
LocalDateTime both = LocalDateTime.now(); // 2026-07-18T14:30:45
```

---

### Q25. How to find the difference between two dates?
**Answer:**
```java
LocalDate start = LocalDate.of(2020, 1, 1);
LocalDate end   = LocalDate.of(2026, 7, 18);

Period period = Period.between(start, end);
System.out.println(period.getYears() + " years, "
    + period.getMonths() + " months, "
    + period.getDays() + " days");
// 6 years, 6 months, 17 days

// Or total days
long totalDays = ChronoUnit.DAYS.between(start, end);
System.out.println(totalDays + " days"); // 2390 days
```

---

## 🔴 Section F – CompletableFuture (Advanced)

---

### Q26. What is CompletableFuture?
**Answer:**
A class that represents a **future result of an async computation**. You can attach callbacks, chain operations, and handle errors without blocking.

```java
CompletableFuture<String> cf = CompletableFuture.supplyAsync(() -> "Hello");
cf.thenApply(String::toUpperCase)
  .thenAccept(System.out::println); // HELLO
```

---

### Q27. Difference between `thenApply()` and `thenCompose()`?
**Answer:**
- `thenApply()` → transforms the result (like `map`)
- `thenCompose()` → chains another CompletableFuture (like `flatMap`)

```java
// thenApply – result is a simple value
cf.thenApply(s -> s.toUpperCase());

// thenCompose – result is another CompletableFuture
cf.thenCompose(s -> fetchFromAPI(s)); // returns CompletableFuture<String>
```

---

### Q28. How to handle errors in CompletableFuture?
**Answer:**
```java
CompletableFuture.supplyAsync(() -> {
        throw new RuntimeException("Error!");
    })
    .exceptionally(ex -> {
        System.out.println("Caught: " + ex.getMessage());
        return "Default";
    })
    .thenAccept(System.out::println); // Default
```

---

### Q29. How to run multiple tasks in parallel?
**Answer:**
```java
CompletableFuture<String> f1 = CompletableFuture.supplyAsync(() -> fetchUser());
CompletableFuture<String> f2 = CompletableFuture.supplyAsync(() -> fetchOrders());
CompletableFuture<String> f3 = CompletableFuture.supplyAsync(() -> fetchPayments());

CompletableFuture.allOf(f1, f2, f3).join(); // wait for ALL

System.out.println(f1.get() + ", " + f2.get() + ", " + f3.get());
```

---

## 🔴 Section G – Collectors (Advanced)

---

### Q30. How to group elements by a property?
**Answer:**
```java
Map<String, List<Employee>> byDept = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDepartment));
```

---

### Q31. Difference between `groupingBy()` and `partitioningBy()`?
**Answer:**
- `groupingBy()` → groups by **any key** (returns `Map<K, List<T>>`)
- `partitioningBy()` → groups by **true/false** (returns `Map<Boolean, List<T>>`)

```java
// groupingBy – multiple groups
Map<String, List<Employee>> byDept = employees.stream()
    .collect(Collectors.groupingBy(e -> e.department));

// partitioningBy – exactly 2 groups (true/false)
Map<Boolean, List<Integer>> evenOdd = nums.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));
```

---

### Q32. How to join strings with a separator using streams?
**Answer:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

String result = names.stream()
    .collect(Collectors.joining(", ", "[", "]"));

System.out.println(result); // [Alice, Bob, Charlie]
```

---

## 🎯 Section H – Coding Challenges (Commonly Asked)

---

### Q33. Sort a list of strings by length.
```java
List<String> words = Arrays.asList("banana", "apple", "kiwi", "cherry");

List<String> sorted = words.stream()
    .sorted(Comparator.comparingInt(String::length))
    .collect(Collectors.toList());

System.out.println(sorted); // [kiwi, apple, banana, cherry]
```

---

### Q34. Find the first non-repeated character in a string.
```java
String input = "aabbcde";

Character result = input.chars()
    .mapToObj(c -> (char) c)
    .collect(Collectors.groupingBy(c -> c, LinkedHashMap::new, Collectors.counting()))
    .entrySet().stream()
    .filter(e -> e.getValue() == 1)
    .map(Map.Entry::getKey)
    .findFirst()
    .orElse(null);

System.out.println(result); // c
```

---

### Q35. Convert a list of strings to a comma-separated uppercase string.
```java
List<String> names = Arrays.asList("alice", "bob", "charlie");

String result = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.joining(", "));

System.out.println(result); // ALICE, BOB, CHARLIE
```

---

### Q36. Find the employee with the highest salary.
```java
Optional<Employee> highest = employees.stream()
    .max(Comparator.comparingDouble(Employee::getSalary));

highest.ifPresent(e -> System.out.println(e.getName()));
```

---

### Q37. Group employees by department and find average salary per department.
```java
Map<String, Double> avgSalary = employees.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        Collectors.averagingDouble(Employee::getSalary)
    ));

System.out.println(avgSalary);
```

---

### Q38. Find all palindrome strings in a list.
```java
List<String> words = Arrays.asList("madam", "hello", "racecar", "world", "level");

List<String> palindromes = words.stream()
    .filter(w -> w.equals(new StringBuilder(w).reverse().toString()))
    .collect(Collectors.toList());

System.out.println(palindromes); // [madam, racecar, level]
```

---

### Q39. Flatten a list of lists and remove duplicates.
```java
List<List<Integer>> nested = Arrays.asList(
    Arrays.asList(1, 2, 3),
    Arrays.asList(3, 4, 5),
    Arrays.asList(5, 6, 7)
);

List<Integer> unique = nested.stream()
    .flatMap(Collection::stream)
    .distinct()
    .sorted()
    .collect(Collectors.toList());

System.out.println(unique); // [1, 2, 3, 4, 5, 6, 7]
```

---

### Q40. Count words in a sentence using streams.
```java
String sentence = "Java is great and Java is powerful";

Map<String, Long> wordCount = Arrays.stream(sentence.split(" "))
    .collect(Collectors.groupingBy(w -> w, Collectors.counting()));

System.out.println(wordCount);
// {Java=2, is=2, great=1, and=1, powerful=1}
```

---

### Q41. Find the sum of squares of even numbers.
```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5, 6);

int sumOfSquares = nums.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * n)
    .reduce(0, Integer::sum);

System.out.println(sumOfSquares); // 4 + 16 + 36 = 56
```

---

### Q42. Convert a Map to a sorted list of entries.
```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 90);
scores.put("Bob", 85);
scores.put("Charlie", 95);

List<Map.Entry<String, Integer>> sorted = scores.entrySet().stream()
    .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
    .collect(Collectors.toList());

sorted.forEach(e -> System.out.println(e.getKey() + ": " + e.getValue()));
// Charlie: 95
// Alice: 90
// Bob: 85
```

---

### Q43. Find the longest string in a list.
```java
List<String> words = Arrays.asList("Java", "Programming", "is", "fun");

Optional<String> longest = words.stream()
    .max(Comparator.comparingInt(String::length));

System.out.println(longest.get()); // Programming
```

---

### Q44. Create a map of name → name length.
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

Map<String, Integer> nameLength = names.stream()
    .collect(Collectors.toMap(n -> n, String::length));

System.out.println(nameLength); // {Alice=5, Bob=3, Charlie=7}
```

---

### Q45. Separate even and odd numbers into two lists.
```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);

Map<Boolean, List<Integer>> split = nums.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));

System.out.println("Even: " + split.get(true));  // [2, 4, 6, 8]
System.out.println("Odd:  " + split.get(false)); // [1, 3, 5, 7]
```

---

## 💡 Quick Revision – One-Liners

| Feature | One-Liner Example |
|---------|------------------|
| Lambda | `(a, b) -> a + b` |
| forEach | `list.forEach(System.out::println)` |
| Filter | `.filter(n -> n > 10)` |
| Map | `.map(String::toUpperCase)` |
| FlatMap | `.flatMap(Collection::stream)` |
| Reduce | `.reduce(0, Integer::sum)` |
| Collect | `.collect(Collectors.toList())` |
| Group | `.collect(Collectors.groupingBy(...))` |
| Join | `.collect(Collectors.joining(", "))` |
| Optional | `Optional.ofNullable(x).orElse("default")` |
| Method Ref | `String::length` |
| CompletableFuture | `CompletableFuture.supplyAsync(() -> "result")` |
| LocalDate | `LocalDate.now().plusDays(7)` |

---

## 📚 Study Order for Interviews
1. ✅ Lambda expressions & functional interfaces
2. ✅ Stream API (filter, map, reduce, collect)
3. ✅ Collectors (groupingBy, partitioningBy, joining)
4. ✅ Optional
5. ✅ Method references
6. ✅ Default & static methods
7. ✅ Date-Time API
8. ✅ CompletableFuture
9. ✅ Coding challenges (practice 5-10 from Section H)

**Good luck with your interviews! 🎉**
