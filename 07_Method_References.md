# 7. Method References

> Method references are a **shorthand for lambdas** when the lambda only calls an existing method. They make code even more readable.

---

## 7.1 What is a Method Reference?

Instead of writing a lambda that just calls a method, you can use `::` syntax.

```java
// Lambda
names.forEach(name -> System.out.println(name));

// Method Reference (same thing, shorter)
names.forEach(System.out::println);
```

> 🔑 **Rule**: If your lambda is `x -> someMethod(x)`, replace it with `ClassName::someMethod`.

---

## 7.2 Four Types of Method References

| Type | Syntax | Lambda Equivalent |
|------|--------|------------------|
| Static method | `ClassName::staticMethod` | `x -> ClassName.staticMethod(x)` |
| Instance method of a specific object | `object::instanceMethod` | `x -> object.instanceMethod(x)` |
| Instance method of an arbitrary object | `ClassName::instanceMethod` | `(x) -> x.instanceMethod()` |
| Constructor | `ClassName::new` | `() -> new ClassName()` |

---

## 7.3 Type 1 – Static Method Reference

```java
// Lambda
Function<String, Integer> parse1 = s -> Integer.parseInt(s);

// Method reference
Function<String, Integer> parse2 = Integer::parseInt;

System.out.println(parse2.apply("123")); // 123
```

### More examples
```java
// Math.abs
UnaryOperator<Integer> abs1 = n -> Math.abs(n);     // lambda
UnaryOperator<Integer> abs2 = Math::abs;              // method reference

// Collections.sort with static comparator
List<Integer> nums = Arrays.asList(3, 1, 2);
nums.sort(Integer::compare);  // static method reference
System.out.println(nums); // [1, 2, 3]
```

---

## 7.4 Type 2 – Instance Method of a Specific Object

```java
String greeting = "Hello, World!";

// Lambda
Supplier<String> upper1 = () -> greeting.toUpperCase();

// Method reference – calling method on a specific object
Supplier<String> upper2 = greeting::toUpperCase;

System.out.println(upper2.get()); // HELLO, WORLD!
```

### Another example
```java
List<String> names = new ArrayList<>();

// Lambda
Consumer<String> add1 = name -> names.add(name);

// Method reference
Consumer<String> add2 = names::add;

add2.accept("Alice");
add2.accept("Bob");
System.out.println(names); // [Alice, Bob]
```

---

## 7.5 Type 3 – Instance Method of an Arbitrary Object

The method is called **on each element** of the stream/collection.

```java
List<String> names = Arrays.asList("alice", "bob", "charlie");

// Lambda
List<String> upper1 = names.stream()
    .map(s -> s.toUpperCase())
    .collect(Collectors.toList());

// Method reference – toUpperCase is called on each String
List<String> upper2 = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

System.out.println(upper2); // [ALICE, BOB, CHARLIE]
```

### More examples
```java
// String::length – calling length() on each string
List<Integer> lengths = names.stream()
    .map(String::length)
    .collect(Collectors.toList());
System.out.println(lengths); // [5, 3, 7]

// String::isEmpty
List<String> words = Arrays.asList("hello", "", "world", "");
long emptyCount = words.stream()
    .filter(String::isEmpty)
    .count();
System.out.println(emptyCount); // 2

// Sorting by natural order
List<String> sorted = names.stream()
    .sorted(String::compareTo)
    .collect(Collectors.toList());
```

---

## 7.6 Type 4 – Constructor Reference

```java
// Lambda
Supplier<List<String>> listMaker1 = () -> new ArrayList<>();

// Constructor reference
Supplier<List<String>> listMaker2 = ArrayList::new;

List<String> myList = listMaker2.get();
myList.add("Java");
System.out.println(myList); // [Java]
```

### With parameters
```java
// Convert String to Integer using constructor
Function<String, Integer> toInt1 = s -> new Integer(s);    // lambda
Function<String, Integer> toInt2 = Integer::new;            // constructor reference

System.out.println(toInt2.apply("42")); // 42
```

### Creating objects from stream
```java
class Person {
    String name;
    Person(String name) { this.name = name; }
    public String toString() { return "Person(" + name + ")"; }
}

List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Create Person objects from names
List<Person> people = names.stream()
    .map(Person::new)           // constructor reference
    .collect(Collectors.toList());

System.out.println(people);
// [Person(Alice), Person(Bob), Person(Charlie)]
```

---

## 7.7 When to Use Method References

| Lambda | Method Reference | Use Reference? |
|--------|-----------------|---------------|
| `s -> System.out.println(s)` | `System.out::println` | ✅ Yes |
| `s -> s.toUpperCase()` | `String::toUpperCase` | ✅ Yes |
| `s -> Integer.parseInt(s)` | `Integer::parseInt` | ✅ Yes |
| `() -> new ArrayList<>()` | `ArrayList::new` | ✅ Yes |
| `s -> s.length() > 5` | ❌ No equivalent | ❌ No (has extra logic) |
| `(a, b) -> a + b` | ❌ No equivalent | ❌ No (operator, not a method) |

> 🔑 **Use method reference when the lambda ONLY calls a single method with no extra logic.**

---

## 7.8 Complete Example

```java
import java.util.*;
import java.util.stream.*;
import java.util.function.*;

public class MethodRefDemo {
    public static void main(String[] args) {

        List<String> names = Arrays.asList("Charlie", "alice", "Bob", "diana");

        // 1. Static method reference – sort case-insensitive
        names.sort(String::compareToIgnoreCase);
        System.out.println(names); // [alice, Bob, Charlie, diana]

        // 2. Instance method of arbitrary object – to uppercase
        List<String> upper = names.stream()
            .map(String::toUpperCase)
            .collect(Collectors.toList());
        System.out.println(upper); // [ALICE, BOB, CHARLIE, DIANA]

        // 3. Instance method of specific object – print
        upper.forEach(System.out::println);

        // 4. Constructor reference – create StringBuilder from each string
        List<StringBuilder> builders = names.stream()
            .map(StringBuilder::new)
            .collect(Collectors.toList());

        builders.forEach(sb -> {
            sb.reverse();
            System.out.println(sb); // ecila, boB, eilrahC, anaid
        });
    }
}
```

---

## ✅ Quick Reference

| Type | Syntax | Example |
|------|--------|---------|
| Static method | `Class::method` | `Integer::parseInt` |
| Specific object's method | `object::method` | `System.out::println` |
| Arbitrary object's method | `Class::method` | `String::toUpperCase` |
| Constructor | `Class::new` | `ArrayList::new` |

---

## ✅ Checklist

- [ ] I can identify when a lambda can be replaced with a method reference
- [ ] I know all 4 types of method references
- [ ] I can use constructor references to create objects in streams
- [ ] I understand the `::` syntax

**Next → [08_DateTime_API.md](./08_DateTime_API.md)**
