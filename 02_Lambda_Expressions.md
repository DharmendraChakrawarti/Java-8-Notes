# 2. Lambda Expressions

> Lambda expressions are the **biggest feature** of Java 8. They let you write shorter, cleaner code by treating functions as values.

---

## 2.1 What is a Lambda?

A **lambda expression** is a short way to write an **anonymous function** (a function without a name).

### Syntax
```
(parameters) -> { body }
```

### Simple Rules
| Rule | Example |
|------|---------|
| One parameter – no parentheses needed | `x -> x * 2` |
| Multiple parameters – use parentheses | `(a, b) -> a + b` |
| One statement – no curly braces needed | `x -> x * 2` |
| Multiple statements – use curly braces | `(a, b) -> { int sum = a + b; return sum; }` |

---

## 2.2 Before vs After Lambda

### Sorting a list – OLD way (Anonymous class)
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

### Sorting a list – NEW way (Lambda)
```java
List<String> names = Arrays.asList("Charlie", "Alice", "Bob");

Collections.sort(names, (a, b) -> a.compareTo(b));

System.out.println(names); // [Alice, Bob, Charlie]
```

> 🔑 **Notice**: 6 lines reduced to 1 line! Same result, much cleaner.

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

        System.out.println("5 + 3 = " + add.operate(5, 3));       // 8
        System.out.println("5 - 3 = " + subtract.operate(5, 3));   // 2
        System.out.println("5 * 3 = " + multiply.operate(5, 3));   // 15
    }
}
```

---

## 2.7 Lambda Scope – Variable Capture

Lambdas can use variables from the **surrounding scope**, but those variables must be **effectively final** (not changed after initialization).

```java
String greeting = "Hello";  // effectively final

Runnable r = () -> System.out.println(greeting + " World!");
r.run(); // Hello World!
```

❌ **This will NOT compile:**
```java
String greeting = "Hello";
greeting = "Hi";  // changed! NOT effectively final

Runnable r = () -> System.out.println(greeting); // COMPILE ERROR
```

---

## 2.8 Common Mistakes

| Mistake | Why It's Wrong |
|---------|---------------|
| Forgetting `@FunctionalInterface` | Not an error but good practice – compiler will check it has exactly 1 abstract method |
| Modifying outer variable inside lambda | Variables used in lambda must be effectively final |
| Using lambda where interface has 2+ abstract methods | Lambda only works with functional interfaces (1 abstract method) |

---

## ✅ Key Takeaways

1. Lambda = short anonymous function → `(params) -> body`
2. Works only with **functional interfaces** (1 abstract method)
3. Reduces boilerplate code dramatically
4. Variables used in lambda must be **effectively final**
5. Lambda is the foundation for **Streams**, **Optional**, and modern Java patterns

**Next → [03_Functional_Interfaces.md](./03_Functional_Interfaces.md)**
