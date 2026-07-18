# 5. Optional Class

> `Optional` is a container that may or may not hold a value. It helps you **avoid NullPointerException** in a clean way.

---

## 5.1 The Problem – NullPointerException

```java
// Old way – this will crash if user is null!
String name = getUser().getName().toUpperCase();
// java.lang.NullPointerException 💥

// Old fix – ugly null checks everywhere
User user = getUser();
if (user != null) {
    String name = user.getName();
    if (name != null) {
        System.out.println(name.toUpperCase());
    }
}
```

> **Optional** solves this by wrapping the value in a container that knows if the value exists or not.

---

## 5.2 Creating Optional

```java
// 1. Optional with a value
Optional<String> name = Optional.of("Dharmendra");

// 2. Optional that might be null
Optional<String> maybe = Optional.ofNullable(null);       // empty
Optional<String> maybe2 = Optional.ofNullable("Hello");   // has value

// 3. Empty Optional
Optional<String> empty = Optional.empty();

// ❌ NEVER do this – will throw NullPointerException
// Optional<String> bad = Optional.of(null);  // CRASH!
```

---

## 5.3 Checking if Value Exists

```java
Optional<String> name = Optional.of("Dharmendra");
Optional<String> empty = Optional.empty();

// isPresent() – returns true if value exists
System.out.println(name.isPresent());   // true
System.out.println(empty.isPresent());  // false

// isEmpty() – Java 11+ (opposite of isPresent)
// System.out.println(empty.isEmpty()); // true

// ifPresent() – run code ONLY if value exists
name.ifPresent(n -> System.out.println("Name: " + n));   // Name: Dharmendra
empty.ifPresent(n -> System.out.println("Name: " + n));  // nothing happens
```

---

## 5.4 Getting the Value

```java
Optional<String> name = Optional.of("Dharmendra");
Optional<String> empty = Optional.empty();

// get() – ⚠️ throws exception if empty
String n = name.get();  // "Dharmendra"
// String x = empty.get();  // NoSuchElementException! 💥

// orElse() – return default value if empty
String n1 = empty.orElse("Unknown");
System.out.println(n1); // "Unknown"

// orElseGet() – lazy default (uses Supplier)
String n2 = empty.orElseGet(() -> "Generated Default");
System.out.println(n2); // "Generated Default"

// orElseThrow() – throw custom exception if empty
String n3 = empty.orElseThrow(() -> new RuntimeException("Value not found!"));
// throws RuntimeException
```

### orElse vs orElseGet – Important Difference!
```java
// orElse – ALWAYS calls the method (even if value exists)
String r1 = name.orElse(getDefault());  // getDefault() is called!

// orElseGet – calls the method ONLY if empty (lazy)
String r2 = name.orElseGet(() -> getDefault());  // getDefault() NOT called

// Use orElseGet when the default value is expensive to create
```

---

## 5.5 Transforming with map() and flatMap()

### map() – Transform the value inside Optional
```java
Optional<String> name = Optional.of("dharmendra");

// Convert to uppercase
Optional<String> upper = name.map(String::toUpperCase);
System.out.println(upper.get()); // "DHARMENDRA"

// Get length
Optional<Integer> length = name.map(String::length);
System.out.println(length.get()); // 10

// If empty, map returns empty
Optional<String> empty = Optional.empty();
Optional<String> result = empty.map(String::toUpperCase);
System.out.println(result.isPresent()); // false
```

### flatMap() – When transformation returns Optional
```java
// Suppose getName() returns Optional<String>
class User {
    private String name;

    public Optional<String> getName() {
        return Optional.ofNullable(name);
    }
}

Optional<User> user = Optional.of(new User());

// ❌ map gives Optional<Optional<String>> – nested!
Optional<Optional<String>> nested = user.map(User::getName);

// ✅ flatMap flattens it to Optional<String>
Optional<String> flat = user.flatMap(User::getName);
```

---

## 5.6 Filtering with filter()

```java
Optional<String> name = Optional.of("Dharmendra");

// Keep value only if it passes the condition
Optional<String> longName = name.filter(n -> n.length() > 5);
System.out.println(longName.isPresent()); // true

Optional<String> shortName = name.filter(n -> n.length() > 20);
System.out.println(shortName.isPresent()); // false
```

---

## 5.7 Chaining Optional Operations

```java
// Real-world example – Safe navigation
class Company {
    Optional<Department> getDepartment() { /* ... */ }
}
class Department {
    Optional<Manager> getManager() { /* ... */ }
}
class Manager {
    Optional<String> getEmail() { /* ... */ }
}

// Get manager's email safely – no null checks!
String email = Optional.of(company)
    .flatMap(Company::getDepartment)
    .flatMap(Department::getManager)
    .flatMap(Manager::getEmail)
    .orElse("no-email@company.com");
```

---

## 5.8 Optional with Streams

```java
// filter a list and get first matching element
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Diana");

Optional<String> firstLongName = names.stream()
    .filter(n -> n.length() > 4)
    .findFirst();

firstLongName.ifPresent(n -> System.out.println("Found: " + n));
// Found: Alice
```

---

## 5.9 Best Practices

| ✅ Do | ❌ Don't |
|-------|---------|
| Use `Optional` as a return type | Use `Optional` for fields or parameters |
| Use `orElse` / `orElseGet` | Call `get()` without checking `isPresent()` |
| Use `map` / `flatMap` for transformations | Use nested `if-else` with `isPresent()` |
| Return `Optional.empty()` instead of `null` | Return `null` from a method that returns `Optional` |
| Use `ifPresent()` for side effects | Use `Optional` in collections like `List<Optional<T>>` |

### ❌ Anti-Pattern
```java
// DON'T do this – defeats the purpose of Optional
Optional<String> name = getName();
if (name.isPresent()) {
    System.out.println(name.get());
} else {
    System.out.println("Unknown");
}

// ✅ DO this instead
String result = getName().orElse("Unknown");
System.out.println(result);
```

---

## 5.10 Complete Example

```java
class Student {
    private String name;
    private String email;

    Student(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public String getName() { return name; }
    public Optional<String> getEmail() { return Optional.ofNullable(email); }
}

public class OptionalDemo {
    public static Optional<Student> findStudent(String name) {
        List<Student> students = Arrays.asList(
            new Student("Alice", "alice@email.com"),
            new Student("Bob", null),
            new Student("Charlie", "charlie@email.com")
        );

        return students.stream()
            .filter(s -> s.getName().equals(name))
            .findFirst();
    }

    public static void main(String[] args) {
        // Find student and get email with fallback
        String email = findStudent("Bob")
            .flatMap(Student::getEmail)
            .map(String::toUpperCase)
            .orElse("NO EMAIL");

        System.out.println(email); // NO EMAIL

        String email2 = findStudent("Alice")
            .flatMap(Student::getEmail)
            .map(String::toUpperCase)
            .orElse("NO EMAIL");

        System.out.println(email2); // ALICE@EMAIL.COM
    }
}
```

---

## ✅ Quick Reference

| Method | Description |
|--------|-------------|
| `Optional.of(value)` | Create with non-null value |
| `Optional.ofNullable(value)` | Create with possibly null value |
| `Optional.empty()` | Create empty Optional |
| `isPresent()` | Check if value exists |
| `ifPresent(Consumer)` | Run action if value exists |
| `get()` | Get value (⚠️ throws if empty) |
| `orElse(default)` | Get value or default |
| `orElseGet(Supplier)` | Get value or lazy default |
| `orElseThrow(Supplier)` | Get value or throw exception |
| `map(Function)` | Transform value |
| `flatMap(Function)` | Transform + flatten |
| `filter(Predicate)` | Keep value if condition matches |

---

## ✅ Checklist

- [ ] I understand why Optional exists (avoid null)
- [ ] I can create Optional using `of`, `ofNullable`, and `empty`
- [ ] I can safely get values using `orElse`, `orElseGet`, `orElseThrow`
- [ ] I can chain operations with `map`, `flatMap`, and `filter`
- [ ] I know the best practices and anti-patterns

**Next → [06_Default_Static_Methods.md](./06_Default_Static_Methods.md)**
