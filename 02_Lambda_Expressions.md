# 2. Lambda Expressions

## What is Lambda?

> **Lambda = A short function without a name.**
> You write what to do in one line instead of creating a whole class.

```
(parameters) -> { body }
```

```java
// Example:
(a, b) -> a + b        // takes two inputs, returns sum
x -> x * 2             // takes one input, returns double
() -> "Hello"           // no input, returns "Hello"
```

---

## Syntax Rules

| Rule | Example |
|------|---------|
| No parameter | `() -> System.out.println("Hi")` |
| One parameter (no brackets needed) | `x -> x * 2` |
| Multiple parameters | `(a, b) -> a + b` |
| One line body (no `{}` needed) | `x -> x * 2` |
| Multi-line body (use `{}` + `return`) | `(a, b) -> { int s = a+b; return s; }` |

---

## Before vs After Lambda

### Sorting — Old Way
```java
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.compareTo(b);
    }
});
```

### Sorting — Lambda Way
```java
Collections.sort(names, (a, b) -> a.compareTo(b));
```

> **6 lines → 1 line. Same result.**

---

## forEach

```java
List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");

// Old
for (String f : fruits) {
    System.out.println(f);
}

// Lambda
fruits.forEach(f -> System.out.println(f));

// Method Reference (shortest)
fruits.forEach(System.out::println);
```

---

## Lambda with Thread

```java
// Old
Thread t = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Running!");
    }
});

// Lambda
Thread t = new Thread(() -> System.out.println("Running!"));
t.start();
```

---

## Lambda with Comparator

```java
List<Integer> nums = Arrays.asList(5, 2, 8, 1, 9);

nums.sort((a, b) -> a - b);  // Ascending: [1, 2, 5, 8, 9]
nums.sort((a, b) -> b - a);  // Descending: [9, 8, 5, 2, 1]
```

---

## Custom Functional Interface

```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

MathOperation add = (a, b) -> a + b;
MathOperation sub = (a, b) -> a - b;
MathOperation mul = (a, b) -> a * b;

System.out.println(add.operate(5, 3));  // 8
System.out.println(sub.operate(5, 3));  // 2
System.out.println(mul.operate(5, 3));  // 15
```

> `@FunctionalInterface` = interface with **exactly 1 abstract method**. Lambda only works with these.

---

## Built-in Functional Interfaces

| Interface | Method | What it does | Example |
|-----------|--------|-------------|---------|
| `Predicate<T>` | `test(T)` | Returns true/false | `n -> n > 0` |
| `Function<T,R>` | `apply(T)` | Converts T to R | `s -> s.length()` |
| `Consumer<T>` | `accept(T)` | Does something, returns nothing | `s -> System.out.println(s)` |
| `Supplier<T>` | `get()` | Takes nothing, returns something | `() -> Math.random()` |

```java
Predicate<Integer> isEven = n -> n % 2 == 0;
System.out.println(isEven.test(4));   // true
System.out.println(isEven.test(7));   // false

Function<String, Integer> len = s -> s.length();
System.out.println(len.apply("Hello"));  // 5

Consumer<String> print = s -> System.out.println(s);
print.accept("Hi!");  // Hi!

Supplier<Double> random = () -> Math.random();
System.out.println(random.get());  // 0.xyz...
```

---

## Variable Capture (Effectively Final)

Lambda can use outside variables, but you **cannot change** them.

```java
String name = "Java";  // effectively final (never changed)
Runnable r = () -> System.out.println(name);  // ✅ Works

String city = "Delhi";
city = "Mumbai";  // changed!
Runnable r2 = () -> System.out.println(city);  // ❌ Compile Error
```

---

## `this` in Lambda vs Anonymous Class

```java
// Lambda    → 'this' = outer class
// Anonymous → 'this' = anonymous class itself
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `x -> return x * 2` | `x -> x * 2` OR `x -> { return x * 2; }` |
| `(int a, b) -> a + b` | `(int a, int b)` OR `(a, b)` |
| Lambda on interface with 2+ methods | Use anonymous class instead |
| Changing variable used in lambda | Make it effectively final |

---

## Key Takeaways

1. Lambda = `(params) -> body` — short anonymous function
2. Works only with **functional interfaces** (1 abstract method)
3. Know 4 built-in interfaces: `Predicate`, `Function`, `Consumer`, `Supplier`
4. Variables must be **effectively final**
5. `this` refers to **outer class**, not the lambda

**Next → [03_Functional_Interfaces.md](./03_Functional_Interfaces.md)**
