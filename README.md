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

### What is the difference between `ArrayList` and `LinkedList`?

| Feature         | ArrayList             | LinkedList                   |
| --------------- | --------------------- | ---------------------------- |
| Data Structure  | Dynamic Array         | Doubly Linked List           |
| Access Time     | Fast (O(1)) for index | Slow (O(n)) for index        |
| Insert/Delete   | Slow in middle (O(n)) | Fast in middle (O(1) if ref) |
| Memory Overhead | Less                  | More (due to node pointers)  |

### Difference Between `HashMap` and `HashSet`

| Feature                  | `HashMap<K, V>`                                                         | `HashSet<E>`                                                |
| ------------------------ | ----------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Purpose**              | Stores **key-value** pairs                                              | Stores **unique elements**                                  |
| **Implements**           | `Map` interface                                                         | `Set` interface                                             |
| **Underlying Structure** | Backed by a `HashTable` (array of buckets)                              | Internally uses a `HashMap`                                 |
| **Data Stored**          | Keys and Values                                                         | Only keys (stored as map keys with a dummy value)           |
| **Duplicates**           | Keys must be unique, values can duplicate                               | No duplicate elements allowed                               |
| **Null Handling**        | Allows 1 `null` key and multiple `null` values                          | Allows 1 `null` element                                     |
| **Example Usage**        | `HashMap<String, Integer> map = new HashMap<>();`<br>`map.put("A", 1);` | `HashSet<String> set = new HashSet<>();`<br>`set.add("A");` |
| **Performance**          | O(1) average for put/get/remove                                         | O(1) average for add/contains/remove                        |
| **Use Case**             | Fast lookup via keys, key-value storage                                 | Unique collection, set operations                           |

### What is the difference between `HashSet` and `TreeSet`?

| Feature       | HashSet                      | TreeSet                              |
| ------------- | ---------------------------- | ------------------------------------ |
| Ordering      | No ordering                  | Elements sorted (natural/comparator) |
| Null Elements | Allows one null              | Does not allow null (throws NPE)     |
| Performance   | Faster (O(1) for add/search) | Slower (O(log n))                    |

### What is the difference between `HashMap` and `Hashtable`?

| Feature          | HashMap                          | Hashtable                  |
| ---------------- | -------------------------------- | -------------------------- |
| Thread Safety    | Not thread-safe                  | Thread-safe (synchronized) |
| Performance      | Better in single-threaded        | Slower due to sync         |
| Null Keys/Values | 1 null key, multiple null values | No null key or value       |

### What is the difference between `Iterator` and `ListIterator`?

* `Iterator`: Works for all collections; only forward traversal; read-remove only.
* `ListIterator`: Only for `List` types; supports **bidirectional traversal**, element modification, and index access.

### What is the fail-fast vs fail-safe iterator?

* **Fail-fast**: Throws `ConcurrentModificationException` if collection is modified during iteration (e.g., `ArrayList`, `HashMap`).
* **Fail-safe**: Works on a cloned copy of the collection, does not throw exception (e.g., `CopyOnWriteArrayList`, `ConcurrentHashMap`).

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

## Java `Function<T, R>`

### What is `Function<T, R>` in Java?

`Function<T, R>` is a **functional interface** in the `java.util.function` package. It represents a function that takes an input of type `T` and returns a result of type `R`.

### What is the method signature of `Function<T, R>`?

```java
R apply(T t);
```

### Give an example usage of `Function`.

```java
Function<String, Integer> strLength = s -> s.length();
System.out.println(strLength.apply("Java")); // Output: 4
```

### How do you compose two `Function`s?

Using the default methods:

* `andThen()`: Applies one function and then another.
* `compose()`: Applies one function **before** another.

```java
Function<Integer, Integer> multiplyBy2 = x -> x * 2;
Function<Integer, Integer> add3 = x -> x + 3;

Function<Integer, Integer> composed = multiplyBy2.andThen(add3);
System.out.println(composed.apply(5)); // Output: 13
```

### What is the difference between `compose()` and `andThen()`?

| Method      | Order of Execution                                     |
| ----------- | ------------------------------------------------------ |
| `compose()` | Applies the **given** function first, then current     |
| `andThen()` | Applies the **current** function first, then the given |

### Can you return a `Function` from a method?

Yes. Since functions are objects in Java 8, you can return them:

```java
public Function<String, String> getUppercaseFunction() {
    return str -> str.toUpperCase();
}
```

### Is `Function<T, R>` serializable?

No, it is **not serializable** by default. You can make it serializable by combining with the `Serializable` interface explicitly.

## Java `Supplier<T>`

### What is a `Supplier<T>` in Java?

A `Supplier<T>` is a **functional interface** that represents a supplier of results. It takes **no input** and **returns a value** of type `T`.

### What is the method signature of `Supplier`?

```java
T get();
```

### Give an example usage of `Supplier`.

```java
Supplier<String> currentTime = () -> LocalTime.now().toString();
System.out.println(currentTime.get());
```

### What are typical use cases of `Supplier`?

* Lazy initialization
* Generating random values
* Supplying test data
* Delayed computation in APIs (e.g., `Optional.orElseGet(Supplier)`)

### How is `Supplier` used with `Optional`?

```java
String value = Optional.ofNullable(null)
    .orElseGet(() -> "Default");

System.out.println(value); // Output: Default
```

### Can a `Supplier` return different values each time?

Yes. A `Supplier` can produce a new result every time `get()` is called.

```java
Supplier<Integer> randomSupplier = () -> new Random().nextInt(100);
System.out.println(randomSupplier.get()); // e.g., 42
System.out.println(randomSupplier.get()); // e.g., 87
```

### What is the difference between `Supplier<T>` and `Function<Void, T>`?

Both represent functions that return a value, but:

* `Supplier<T>` is **specifically designed** for no-arg producers.
* `Function<Void, T>` is syntactically awkward and not idiomatic in Java 8+.

## Java Predicate

### What is a `Predicate` in Java?

A `Predicate<T>` is a **functional interface** introduced in Java 8 (in `java.util.function` package) that takes an input of type `T` and returns a `boolean` result. It's commonly used for filtering, matching, and testing conditions in a **functional style**.

### What is the method signature of the `Predicate` interface?

```java
boolean test(T t);
```

This method evaluates a condition (predicate) on the given argument and returns `true` or `false`.

### How do you use `Predicate` with Java Streams?

You typically use it with the `filter()` method:

```java
List<String> names = List.of("Aman", "Harsh", "Anil");
Predicate<String> startsWithA = name -> name.startsWith("A");

List<String> filtered = names.stream()
    .filter(startsWithA)
    .collect(Collectors.toList());
```

**Output:** `[Aman, Anil]`

### Can you combine two predicates? How?

Yes. The `Predicate` interface provides **default methods**:

* `and()`
* `or()`
* `negate()`

```java
Predicate<String> startsWithA = s -> s.startsWith("A");
Predicate<String> endsWithE = s -> s.endsWith("e");

Predicate<String> startsAndEnds = startsWithA.and(endsWithE);
```

### What does the `negate()` method do in a Predicate?

It returns a new predicate that represents the **logical negation** of the current predicate.

```java
Predicate<Integer> isEven = x -> x % 2 == 0;
Predicate<Integer> isOdd = isEven.negate();

System.out.println(isOdd.test(3)); // true
```

### Can you pass a `Predicate` to a method?

Yes. Since it's a functional interface, you can pass it as a lambda or method reference.

```java
public static void filterNumbers(List<Integer> numbers, Predicate<Integer> condition) {
    numbers.stream().filter(condition).forEach(System.out::println);
}

filterNumbers(List.of(1, 2, 3, 4), n -> n % 2 == 0); // prints 2 and 4
```

### How is `Predicate<T>` different from `Function<T, Boolean>`?

Both return a `Boolean`, but:

* `Predicate<T>` is **specifically designed for boolean-valued conditions** and provides extra logical-composition methods (`and`, `or`, `negate`).
* `Function<T, Boolean>` is more general and does **not provide** logical composition out-of-the-box.

### Can you chain multiple `Predicate`s in a single stream?

Yes. Example:

```java
Predicate<String> startsWithA = s -> s.startsWith("A");
Predicate<String> lengthIs5 = s -> s.length() == 5;

List<String> filtered = names.stream()
    .filter(startsWithA.and(lengthIs5))
    .collect(Collectors.toList());
```

### Is `Predicate` serializable?

No, `Predicate` is **not serializable** by default. If you need serialization, you need to implement a custom `Predicate` that extends both `Predicate` and `Serializable`.

### How would you write a custom reusable predicate?

```java
public class MyPredicates {
    public static Predicate<String> isNullOrEmpty() {
        return s -> s == null || s.isEmpty();
    }
}
```

Usage:

```java
Predicate<String> check = MyPredicates.isNullOrEmpty();
```

### Predicate Method List

| Method            | Description                    |
| ----------------- | ------------------------------ |
| `test(T t)`       | Evaluates the predicate        |
| `and(Predicate)`  | Logical AND of two predicates  |
| `or(Predicate)`   | Logical OR of two predicates   |
| `negate()`        | Logical NOT of the predicate   |
| `isEqual(Object)` | Static method to test equality |

### Summary Table

| Interface       | Input | Output  | Method    | Use Case                        |
| --------------- | ----- | ------- | --------- | ------------------------------- |
| `Predicate<T>`  | T     | boolean | `test()`  | Filtering, matching             |
| `Function<T,R>` | T     | R       | `apply()` | Transformation                  |
| `Supplier<T>`   | -     | T       | `get()`   | Value supplier, lazy evaluation |

## Java 8 Stream API

### What are Streams in Java?

Streams represent a **sequence of elements** supporting **sequential and parallel** operations using **functional-style programming**. They don't store data but operate on source collections.

### Difference between `map()` and `flatMap()`?

* `map()`: Transforms each element.
* `flatMap()`: Flattens nested structures and then applies transformation.

**Example:**

```java
// map example
List<String> names = list.stream().map(user -> user.getName()).collect(Collectors.toList());

// flatMap example
List<String> allWords = sentences.stream()
    .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
    .collect(Collectors.toList());
```

### What is lazy evaluation in Stream API?

Streams are lazy - intermediate operations like `map()` or `filter()` **don't execute** until a **terminal operation** like `collect()` or `forEach()` is invoked. This improves performance by avoiding unnecessary computation.

### What is the difference between intermediate and terminal operations?

* **Intermediate**: Return another stream (e.g., `filter()`, `map()`, `sorted()`)
* **Terminal**: Triggers computation (e.g., `collect()`, `forEach()`, `count()`)

### How to perform parallel processing with streams?

Use `parallelStream()` instead of `stream()`.

```java
list.parallelStream().forEach(item -> process(item));
```

Note: Use with caution due to thread-safety and ordering concerns.

### Can Stream be reused?

No. Once a terminal operation is called, the stream is **consumed** and cannot be reused. You must create a new stream.

### How do you count the number of strings in a list that start with a specific letter (e.g., "A")?

```java
List<String> names = List.of("Aman", "Bob", "Adam", "Ankit", "Brian");

long count = names.stream()
                  .filter(name -> name.startsWith("A"))
                  .count();

System.out.println(count); // Output: 3
```

### How do you find the maximum number in a list using Stream?

```java
List<Integer> numbers = List.of(10, 25, 3, 47, 15);

int max = numbers.stream()
                 .max(Integer::compare)
                 .orElseThrow();

System.out.println(max); // Output: 47
```

### How do you convert a list of strings to uppercase using Stream API?

```java
List<String> names = List.of("Akasht", "doe", "Aman");

List<String> upper = names.stream()
                          .map(String::toUpperCase)
                          .collect(Collectors.toList());

System.out.println(upper); // [Akasht, DOE, Aman]
```

### How do you remove duplicates from a list using Stream?

```java
List<Integer> numbers = List.of(1, 2, 2, 3, 4, 4, 5);

List<Integer> unique = numbers.stream()
                              .distinct()
                              .collect(Collectors.toList());

System.out.println(unique); // [1, 2, 3, 4, 5]
```

### How to group a list of strings by their first character?

```java
List<String> names = List.of("apple", "banana", "apricot", "blueberry");

Map<Character, List<String>> grouped = names.stream()
    .collect(Collectors.groupingBy(s -> s.charAt(0)));

System.out.println(grouped);
// Output: {a=[apple, apricot], b=[banana, blueberry]}
```

### How do you join a list of strings with commas using Stream API?

```java
List<String> words = List.of("Java", "Python", "Go");

String result = words.stream()
                     .collect(Collectors.joining(", "));

System.out.println(result); // Output: Java, Python, Go
```

### How do you calculate the sum of all even numbers in a list?

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

int sum = numbers.stream()
                 .filter(n -> n % 2 == 0)
                 .mapToInt(Integer::intValue)
                 .sum();

System.out.println(sum); // Output: 12
```

### How do you sort a list of strings in reverse alphabetical order?

```java
List<String> names = List.of("Apple", "Mango", "Banana");

List<String> sorted = names.stream()
                           .sorted(Comparator.reverseOrder())
                           .collect(Collectors.toList());

System.out.println(sorted); // [Mango, Banana, Apple]
```

### How do you filter a list of objects based on a field?

```java
class Employee {
    String name;
    int salary;

    Employee(String name, int salary) {
        this.name = name;
        this.salary = salary;
    }
}

List<Employee> employees = List.of(
    new Employee("Akasht", 4000),
    new Employee("Akash", 6000),
    new Employee("Mark", 7000)
);

List<Employee> highEarners = employees.stream()
    .filter(e -> e.salary > 5000)
    .collect(Collectors.toList());
```

### How do you count the frequency of elements in a list?

```java
List<String> items = List.of("apple", "banana", "apple", "orange", "banana", "apple");

Map<String, Long> freq = items.stream()
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

System.out.println(freq);
// Output: {orange=1, banana=2, apple=3}
```

### How do you partition a list into two based on a condition?

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

Map<Boolean, List<Integer>> partitioned = numbers.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));

System.out.println(partitioned);
// Output: {false=[1, 3, 5], true=[2, 4, 6]}
```

### Count the occurrence of elements using Stream API and output in sorted order of the key

```java
List<String> items = List.of("apple", "banana", "apple", "orange", "banana", "apple");

Map<String, Long> result = items.stream()
    .collect(Collectors.groupingBy(Function.identity(), TreeMap::new, Collectors.counting()));

System.out.println(result);
// Output: {apple=3, banana=2, orange=1}
```

### Find the first non-repeating character in a string using Stream API

```java
String input = "aabbcdeff";

Optional<Character> firstNonRepeating = input.chars()
    .mapToObj(c -> (char) c)
    .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting()))
    .entrySet().stream()
    .filter(e -> e.getValue() == 1)
    .map(Map.Entry::getKey)
    .findFirst();

System.out.println(firstNonRepeating.orElse(null)); // Output: c
```

### Flatten a list of lists using Stream API

```java
List<List<String>> nested = List.of(
    List.of("a", "b"),
    List.of("c", "d"),
    List.of("e")
);

List<String> flatList = nested.stream()
    .flatMap(List::stream)
    .collect(Collectors.toList());

System.out.println(flatList); // Output: [a, b, c, d, e]
```

### Get the top 3 highest numbers from a list

```java
List<Integer> numbers = List.of(5, 3, 9, 1, 4, 7);

List<Integer> top3 = numbers.stream()
    .sorted(Comparator.reverseOrder())
    .limit(3)
    .collect(Collectors.toList());

System.out.println(top3); // Output: [9, 7, 5]
```

### Sort a map by its values in descending order using Stream API

```java
Map<String, Integer> map = Map.of("apple", 3, "banana", 2, "orange", 5);

LinkedHashMap<String, Integer> sortedByValue = map.entrySet().stream()
    .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
    .collect(Collectors.toMap(
        Map.Entry::getKey,
        Map.Entry::getValue,
        (e1, e2) -> e1,
        LinkedHashMap::new
    ));

System.out.println(sortedByValue);
// Output: {orange=5, apple=3, banana=2}
```

### Group a list of employees by department

```java
class Employee {
    String name;
    String dept;
    Employee(String name, String dept) {
        this.name = name;
        this.dept = dept;
    }
}

List<Employee> employees = List.of(
    new Employee("Akasht", "IT"),
    new Employee("Akash", "HR"),
    new Employee("Aman", "IT")
);

Map<String, List<Employee>> grouped = employees.stream()
    .collect(Collectors.groupingBy(emp -> emp.dept));

grouped.forEach((k, v) -> System.out.println(k + " => " + v.stream().map(e -> e.name).toList()));
// Output:
// IT => [Akasht, Aman]
// HR => [Akash]
```

### Find duplicate elements in a list using Streams

```java
List<String> items = List.of("apple", "banana", "apple", "orange", "banana", "apple");

Set<String> duplicates = items.stream()
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
    .entrySet().stream()
    .filter(e -> e.getValue() > 1)
    .map(Map.Entry::getKey)
    .collect(Collectors.toSet());

System.out.println(duplicates); // Output: [apple, banana]
```

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

### Is it valid to use only `try` and `finally` without a `catch` block?

Yes, it is **completely valid** to use `try` with `finally` without a `catch` block.

This is useful when:

* You want to **ensure resource cleanup**, regardless of whether an exception occurs or not.
* You're handling exceptions elsewhere (e.g., with `throws`) but still want to release resources.

**Example:**

```java
public void writeToFile(String filePath) throws IOException {
    FileWriter writer = null;
    try {
        writer = new FileWriter(filePath);
        writer.write("Hello, World!");
    } finally {
        if (writer != null) {
            writer.close(); // Ensures resource is released
        }
    }
}
```

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