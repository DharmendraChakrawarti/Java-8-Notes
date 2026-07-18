# 10. CompletableFuture & Concurrency

> `CompletableFuture` is Java 8's way to write **asynchronous, non-blocking** code. Think of it as a "promise" that a result will be available in the future.

---

## 10.1 The Problem – Blocking Code

```java
// Old way – blocks the main thread
String data = fetchDataFromAPI();       // waits 3 seconds ⏳
String processed = processData(data);   // waits 2 seconds ⏳
saveToDatabase(processed);              // waits 1 second  ⏳
// Total: 6 seconds of WAITING 😴
```

> With `CompletableFuture`, operations can run **in the background** while your program continues.

---

## 10.2 Creating CompletableFuture

### supplyAsync() – Run task that returns a value
```java
import java.util.concurrent.CompletableFuture;

CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    // This runs in a background thread
    System.out.println("Running in: " + Thread.currentThread().getName());
    return "Hello from background!";
});

// Get the result (blocks until ready)
String result = future.get();
System.out.println(result); // Hello from background!
```

### runAsync() – Run task that returns nothing
```java
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    System.out.println("Background task running...");
    // do some work
});

future.get(); // wait for it to finish
```

### completedFuture() – Already completed
```java
CompletableFuture<String> done = CompletableFuture.completedFuture("Already done!");
System.out.println(done.get()); // Already done!
```

---

## 10.3 Chaining Operations

### thenApply() – Transform the result (like map)
```java
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> "hello")
    .thenApply(s -> s.toUpperCase())
    .thenApply(s -> s + " WORLD!");

System.out.println(future.get()); // HELLO WORLD!
```

### thenAccept() – Consume the result (no return)
```java
CompletableFuture.supplyAsync(() -> "Hello")
    .thenAccept(result -> System.out.println("Got: " + result));
// Got: Hello
```

### thenRun() – Run something after (no access to result)
```java
CompletableFuture.supplyAsync(() -> "Hello")
    .thenRun(() -> System.out.println("Task completed!"));
// Task completed!
```

### Chain Summary
| Method | Input | Output | Use When |
|--------|-------|--------|----------|
| `thenApply(Function)` | Result | New result | Transform the value |
| `thenAccept(Consumer)` | Result | void | Use the value |
| `thenRun(Runnable)` | Nothing | void | Just run next task |

---

## 10.4 Combining Multiple Futures

### thenCombine() – Combine two results
```java
CompletableFuture<Integer> price = CompletableFuture.supplyAsync(() -> {
    // fetch price from API
    return 100;
});

CompletableFuture<Integer> discount = CompletableFuture.supplyAsync(() -> {
    // fetch discount from API
    return 20;
});

// Combine both results
CompletableFuture<Integer> finalPrice = price.thenCombine(discount,
    (p, d) -> p - d
);

System.out.println("Final price: " + finalPrice.get()); // 80
```

### thenCompose() – Chain dependent futures (like flatMap)
```java
// Second future depends on the first result
CompletableFuture<String> result = CompletableFuture
    .supplyAsync(() -> "user123")
    .thenCompose(userId -> fetchUserDetails(userId));  // returns another CF

// fetchUserDetails is like:
CompletableFuture<String> fetchUserDetails(String userId) {
    return CompletableFuture.supplyAsync(() -> "Details of " + userId);
}
```

### allOf() – Wait for ALL futures to complete
```java
CompletableFuture<String> f1 = CompletableFuture.supplyAsync(() -> "Result 1");
CompletableFuture<String> f2 = CompletableFuture.supplyAsync(() -> "Result 2");
CompletableFuture<String> f3 = CompletableFuture.supplyAsync(() -> "Result 3");

CompletableFuture<Void> allDone = CompletableFuture.allOf(f1, f2, f3);

allDone.thenRun(() -> {
    try {
        System.out.println(f1.get()); // Result 1
        System.out.println(f2.get()); // Result 2
        System.out.println(f3.get()); // Result 3
    } catch (Exception e) {
        e.printStackTrace();
    }
});
```

### anyOf() – Wait for FIRST future to complete
```java
CompletableFuture<Object> fastest = CompletableFuture.anyOf(f1, f2, f3);

System.out.println("First result: " + fastest.get());
```

---

## 10.5 Error Handling

### exceptionally() – Handle errors
```java
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> {
        if (true) throw new RuntimeException("Something went wrong!");
        return "Success";
    })
    .exceptionally(ex -> {
        System.out.println("Error: " + ex.getMessage());
        return "Default Value";
    });

System.out.println(future.get()); // Default Value
```

### handle() – Handle both success and error
```java
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> {
        // might succeed or fail
        return "Success!";
    })
    .handle((result, ex) -> {
        if (ex != null) {
            return "Error: " + ex.getMessage();
        }
        return "Result: " + result;
    });

System.out.println(future.get()); // Result: Success!
```

### whenComplete() – Do something after (both cases)
```java
CompletableFuture.supplyAsync(() -> "Hello")
    .whenComplete((result, ex) -> {
        if (ex != null) {
            System.out.println("Failed: " + ex.getMessage());
        } else {
            System.out.println("Succeeded: " + result);
        }
    });
```

---

## 10.6 Async Variants

Every chaining method has an `Async` version that runs on a **different thread**:

```java
// thenApply    → runs on SAME thread as previous stage
// thenApplyAsync → runs on DIFFERENT thread

CompletableFuture.supplyAsync(() -> "Hello")
    .thenApplyAsync(s -> {
        System.out.println("Thread: " + Thread.currentThread().getName());
        return s.toUpperCase();
    })
    .thenAcceptAsync(s -> {
        System.out.println("Thread: " + Thread.currentThread().getName());
        System.out.println(s);
    });
```

---

## 10.7 Timeouts (Java 9+, but useful to know)

```java
// Java 9+
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> {
        sleep(5000); // simulate slow task
        return "Done";
    })
    .orTimeout(3, TimeUnit.SECONDS)  // throw exception after 3s
    .exceptionally(ex -> "Timed out!");
```

---

## 10.8 Real-world Example – Parallel API Calls

```java
public class ParallelAPICalls {

    // Simulate API calls
    static CompletableFuture<String> fetchUserName() {
        return CompletableFuture.supplyAsync(() -> {
            sleep(1000); // 1 second
            return "Dharmendra";
        });
    }

    static CompletableFuture<String> fetchUserEmail() {
        return CompletableFuture.supplyAsync(() -> {
            sleep(1500); // 1.5 seconds
            return "dharm@email.com";
        });
    }

    static CompletableFuture<Integer> fetchOrderCount() {
        return CompletableFuture.supplyAsync(() -> {
            sleep(800); // 0.8 seconds
            return 42;
        });
    }

    public static void main(String[] args) throws Exception {
        long start = System.currentTimeMillis();

        // Run ALL three API calls in PARALLEL
        CompletableFuture<String> nameFuture  = fetchUserName();
        CompletableFuture<String> emailFuture = fetchUserEmail();
        CompletableFuture<Integer> orderFuture = fetchOrderCount();

        // Wait for all and combine
        CompletableFuture.allOf(nameFuture, emailFuture, orderFuture).join();

        String name  = nameFuture.get();
        String email = emailFuture.get();
        int orders   = orderFuture.get();

        long elapsed = System.currentTimeMillis() - start;

        System.out.println("Name:   " + name);
        System.out.println("Email:  " + email);
        System.out.println("Orders: " + orders);
        System.out.println("Time:   " + elapsed + "ms");
        // Time: ~1500ms (NOT 3300ms!) because they ran in parallel! 🚀
    }

    static void sleep(long ms) {
        try { Thread.sleep(ms); } catch (InterruptedException e) { }
    }
}
```

---

## 10.9 CompletableFuture vs Future

| Feature | `Future` (old) | `CompletableFuture` (Java 8) |
|---------|---------------|------------------------------|
| Get result | `get()` (blocking) | `get()` + callbacks |
| Chain operations | ❌ No | ✅ `thenApply`, `thenCompose` |
| Combine futures | ❌ No | ✅ `thenCombine`, `allOf` |
| Error handling | ❌ No | ✅ `exceptionally`, `handle` |
| Non-blocking callbacks | ❌ No | ✅ `thenAccept`, `whenComplete` |
| Complete manually | ❌ No | ✅ `complete()`, `completeExceptionally()` |

---

## ✅ Methods Cheat Sheet

| Method | What It Does |
|--------|-------------|
| `supplyAsync(Supplier)` | Run task that returns value |
| `runAsync(Runnable)` | Run task, no return |
| `thenApply(Function)` | Transform result |
| `thenAccept(Consumer)` | Use result |
| `thenRun(Runnable)` | Run after completion |
| `thenCombine(CF, BiFunction)` | Combine two results |
| `thenCompose(Function)` | Chain dependent futures |
| `allOf(CF...)` | Wait for all |
| `anyOf(CF...)` | Wait for first |
| `exceptionally(Function)` | Handle error |
| `handle(BiFunction)` | Handle success + error |
| `join()` | Get result (unchecked exception) |

---

## ✅ Checklist

- [ ] I can create async tasks with `supplyAsync` and `runAsync`
- [ ] I can chain operations with `thenApply`, `thenAccept`, `thenRun`
- [ ] I can combine futures with `thenCombine` and `allOf`
- [ ] I can handle errors with `exceptionally` and `handle`
- [ ] I understand the difference between `Future` and `CompletableFuture`

**Next → [11_Interview_Questions.md](./11_Interview_Questions.md)**
