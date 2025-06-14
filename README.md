# Java Interview Questions (3-5 Years Experience)

This repository contains a hand-picked list of **Java interview questions and answers** commonly asked in interviews for developers with 3-5 years of experience. Questions focus on **core concepts, performance, real-world use cases**, and **best practices** in Java.

## Core Java

### What is the difference between `==` and `equals()` in Java?
  
- `==` compares object references (memory addresses).  
- `equals()` compares object content.  
For example, two `String` objects with the same value will return `true` for `equals()` but may return `false` for `==` if not interned.

### What are the access modifiers in Java and their scope?

- `private`: Within the same class  
- `default` (no modifier): Within the same package  
- `protected`: Same package + subclasses  
- `public`: Accessible everywhere

### What is the purpose of the `final` keyword in Java?

- `final` variable: value cannot be changed  
- `final` method: cannot be overridden  
- `final` class: cannot be extended

## Collections Framework

### What is the difference between `HashMap`, `TreeMap`, and `LinkedHashMap`?

- `HashMap`: Unordered, allows one null key  
- `LinkedHashMap`: Maintains insertion order  
- `TreeMap`: Sorted order (natural or comparator), no null key

### How does `HashMap` work internally?

- Uses an array of buckets.
- Hashcode of key determines bucket index.
- Uses `equals()` to resolve collisions (via chaining).
- Java 8+: Converts buckets with many entries to balanced trees.

### What are `ConcurrentHashMap` advantages over `HashMap`?

- Thread-safe without needing external synchronization.
- Uses segment locking or bucket-level locking for performance.
- Prevents `ConcurrentModificationException`.

## Java 8 Features

### What is a Functional Interface?

An interface with only one abstract method.  
Examples: `Runnable`, `Callable`, `Comparator`, `Function<T, R>`  
Can be used with lambda expressions.

### How does the Stream API differ from collections?

- Streams are **not data structures**, but views on data.
- Support **functional-style operations** (map, filter, reduce).
- **Lazy evaluation** improves performance.
- **Parallel streams** utilize multiple threads for large datasets.

### What is `Optional` in Java?

A container object that may or may not contain a non-null value.  
Used to avoid `NullPointerException`.  
Example: `Optional.of(value)`, `Optional.empty()`, `Optional.orElse()`

## Exceptions in Java

### What is the difference between checked and unchecked exceptions?

* **Checked exceptions**: Subclass of `Exception` (excluding `RuntimeException`), must be declared using `throws` or handled in a `try-catch`. Example: `IOException`, `SQLException`.
* **Unchecked exceptions**: Subclass of `RuntimeException`, not checked at compile time. Example: `NullPointerException`, `IndexOutOfBoundsException`.

### Can you create a custom exception in Java?
Yes, by extending `Exception` or `RuntimeException`.

```java
public class MyCustomException extends Exception {
    public MyCustomException(String message) {
        super(message);
    }
}
```

### What happens if an exception is not caught?
If an exception is not caught, it propagates up the call stack. If uncaught at the top level, the JVM terminates the program and prints the stack trace.

## try, catch and finally block in Java

### Will `finally` block always execute?
Yes, **unless**:

* JVM exits using `System.exit()`
* The thread is forcibly killed
* The program crashes

Even if an exception is thrown or a return statement exists in the `try` or `catch`, `finally` will still execute.

### What happens when both `catch` and `finally` have return statements?
The `finally` block's return will override any return from `try` or `catch`.

```java
public int test() {
    try {
        return 1;
    } catch (Exception e) {
        return 2;
    } finally {
        return 3; // This will be returned
    }
}
```

## final, finally and finalize in Java

### What is the difference between `final`, `finally`, and `finalize()`?

| Keyword      | Purpose                                                                |
| ------------ | ---------------------------------------------------------------------- |
| `final`      | Used to declare constants, prevent method overriding or inheritance    |
| `finally`    | Block that always executes after `try-catch`                           |
| `finalize()` | Method invoked by GC before object destruction (Deprecated in Java 9+) |

### Can you override a `finalize()` method?
Yes, but it's **not recommended**. Also, it's deprecated starting from Java 9 and removed in later versions. Use `java.lang.ref.Cleaner` or try-with-resources instead.

## How `hashCode()` and `equals()` are linked in Java

### Why should you override `hashCode()` when overriding `equals()`?
Because Java's collections (e.g., `HashMap`, `HashSet`) use **`hashCode()` to find the bucket** and **`equals()` to check object equality**. If `equals()` is overridden and `hashCode()` isn't, inconsistent behavior may result in object not being found in a collection.

### What are the contracts of `hashCode()` and `equals()`?

* If two objects are equal (`equals()` returns true), they **must** have the same `hashCode()`.
* If two objects have the same `hashCode()`, they **may or may not** be equal.

### Sample implementation for both:

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;
    if (obj == null || getClass() != obj.getClass()) return false;
    MyClass other = (MyClass) obj;
    return id == other.id;
}

@Override
public int hashCode() {
    return Objects.hash(id);
}
```

## Multithreading in Java

### What is the difference between `Runnable` and `Callable`?

* `Runnable`: Does not return a result, cannot throw checked exceptions.
* `Callable<V>`: Returns a result (`V`) and can throw checked exceptions. Used with `ExecutorService`.

### How do you prevent thread-safety issues in Java?

* Use `synchronized` blocks or methods
* Use thread-safe collections (e.g., `ConcurrentHashMap`)
* Use locks (`ReentrantLock`, `ReadWriteLock`)
* Use atomic classes (`AtomicInteger`, etc.)

### What is thread starvation?
Occurs when lower-priority threads are **blocked indefinitely** because high-priority threads occupy all available CPU time or resources.

### What are the benefits of the `ExecutorService`?

* Thread pool management
* Task queuing
* Resource optimization
* Graceful shutdown
  
Example:

```java
ExecutorService executor = Executors.newFixedThreadPool(5);
executor.submit(() -> System.out.println("Task"));
executor.shutdown();
```

### Difference between `synchronized` and `ReentrantLock`?

- `synchronized` is simpler and built-in but less flexible.
- `ReentrantLock` provides:
  - Try lock
  - Timed lock
  - Interruptible lock
  - Fairness policies

### What are `volatile` and `atomic` variables?

- `volatile`: Ensures visibility (not atomicity) of changes across threads.
- Atomic variables (`AtomicInteger`, etc.): Provide lock-free thread-safe operations.

### What is the Executor Framework?

A high-level replacement for creating threads manually.  
Components:
- `ExecutorService`
- `ThreadPoolExecutor`
- `ScheduledExecutorService`  
Manages a pool of threads and handles queuing and scheduling.

## transient keyword in Java

### What is the use of the `transient` keyword?
Marks a field **not to be serialized** when using Java's built-in serialization mechanism. Helps in excluding sensitive or non-serializable fields.

### Example of `transient` keyword

```java
class User implements Serializable {
    private String username;
    private transient String password; // Will not be saved
}
```

### When to use `transient`?

* For **sensitive data** (passwords, tokens)
* For **non-serializable** fields (e.g., `Thread`, `Socket`)
* For **performance optimization** if the field can be reconstructed

## JVM and Performance

### What are memory areas in JVM?

- Heap: Stores objects
- Stack: Stores method calls and local variables
- Method Area: Stores class-level metadata
- PC Register: Keeps track of current thread execution
- Native Method Stack: For native code execution

### How does garbage collection work in Java?

Java uses generational GC (Young, Old, and sometimes Permanent Generation).  
Collectors:
- Serial, Parallel, CMS (deprecated), G1, ZGC, Shenandoah

### How to analyze memory leaks in Java?

- Use profiling tools: JVisualVM, YourKit, Eclipse MAT
- Monitor heap usage and GC logs
- Look for classes holding references too long (e.g., caches, static maps)

## Design Patterns (Java)

### Explain the Singleton Pattern with thread safety.

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized(Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

### When to use Strategy Pattern?

When you want to define a family of algorithms, encapsulate each one, and make them interchangeable. Used for decoupling algorithm from the object using it.

### What is the difference between Factory and Abstract Factory Pattern?

* Factory: Creates objects without exposing the creation logic.
* Abstract Factory: Creates families of related or dependent objects without specifying their concrete classes.

## Miscellaneous

### What is serialization? What is the role of `serialVersionUID`?

* Serialization is the process of converting an object into a byte stream.
* `serialVersionUID` ensures version compatibility during deserialization.

### What is the difference between `String`, `StringBuilder`, and `StringBuffer`?

* `String`: Immutable
* `StringBuilder`: Mutable, not thread-safe
* `StringBuffer`: Mutable, thread-safe (synchronized methods)