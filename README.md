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

- `final variable:` value cannot be changed  
- `final method:` cannot be overridden  
- `final class:` cannot be extended

### What are wrapper classes in Java?

Wrapper classes are object representations of primitive data types.
Each primitive has a corresponding wrapper:

| Primitive | Wrapper Class |
| --------- | ------------- |
| `byte`    | `Byte`        |
| `short`   | `Short`       |
| `int`     | `Integer`     |
| `long`    | `Long`        |
| `float`   | `Float`       |
| `double`  | `Double`      |
| `char`    | `Character`   |
| `boolean` | `Boolean`     |

They are used when objects are required instead of primitives - for example, in collections (`List<Integer>`, not `List<int>`).

### What is autoboxing and unboxing?

* **Autoboxing**: Automatic conversion of primitive to its corresponding wrapper object.
  Example:

  ```java
  Integer i = 10; // int -> Integer
  ```

* **Unboxing**: Automatic conversion of wrapper to its primitive type.
  Example:

  ```java
  int j = new Integer(20); // Integer -> int
  ```

Introduced in **Java 5** to simplify conversions.

### Why are wrapper objects immutable?

Wrapper classes are **immutable** because:

* Once created, their value cannot be changed.
* They are often used in **caching** and **multithreaded** environments.
* Ensures **safety** when used as keys in `Map`, elements in `Set`.

### What is the difference between `==` and `.equals()` with wrapper classes?


* `==` checks **reference equality**
* `.equals()` checks **value equality**

Example:

```java
Integer a = 100;
Integer b = 100;
System.out.println(a == b);       // true (within cache range)
System.out.println(a.equals(b));  // true

Integer x = 128;
Integer y = 128;
System.out.println(x == y);       // false (outside cache range)
System.out.println(x.equals(y));  // true
```

### What is Integer caching in Java?

Java **caches Integer objects** in the range **-128 to 127**.
When autoboxing values in this range, the **same object** is returned.

This applies to `Byte`, `Short`, `Integer`, `Long`, and `Character`.

### Can we store `null` in a wrapper object? What happens on unboxing?

Yes, wrapper objects can be `null`, but unboxing a `null` will throw a **`NullPointerException`**.

Example:

```java
Integer i = null;
int x = i; //NullPointerException at runtime
```

### How does autoboxing affect performance?

* Frequent boxing/unboxing can lead to **increased memory usage** and **performance overhead**.
* Especially problematic in **loops**, **streams**, or **high-performance applications**.
* Prefer primitives where possible in **hot paths**.

### Can wrapper classes be extended?

**No**. All wrapper classes in Java (`Integer`, `Double`, etc.) are **`final`**, so they **cannot be subclassed**.

### What will be the output of the following code?

```java
Integer a = 128;
Integer b = 128;
System.out.println(a == b);
```

Output: `false`
Because 128 is **outside the cache range** (-128 to 127), two different objects are created.

### Is autoboxing applicable to method arguments?

**Yes**. Java will automatically box/unbox arguments when calling methods.

```java
public void print(Integer i) {
    System.out.println(i);
}

print(5); // int is autoboxed to Integer
```

## Immutability in Java

### What is an immutable class in Java?

An **immutable class** is one whose **state cannot be changed after it is created**. Once an object is initialized, we **cannot modify** its fields.

Examples:

* `String`
* `Integer`, `Boolean`, `BigDecimal`, `LocalDate`, etc.

### What are the benefits of immutability?

* **Thread safety** - no synchronization needed
* **Predictable behavior** - ideal for caching and functional programming
* **Safe in collections** - no risk of corruption or inconsistency
* **Ease of testing** - no hidden state changes

### How do we create an immutable class in Java?

To create an immutable class:

1. Declare the class as `final`
2. Make all fields `private` and `final`
3. No setters
4. Initialize fields only via constructor
5. Return **copies** of mutable objects (defensive copy)

### Example: Immutable `Person` class

```java
public final class Person {
    private final String name;
    private final int age;
    private final List<String> hobbies;

    public Person(String name, int age, List<String> hobbies) {
        this.name = name;
        this.age = age;
        // Defensive copy for mutable list
        this.hobbies = new ArrayList<>(hobbies);
    }

    public String getName() { return name; }
    public int getAge() { return age; }

    public List<String> getHobbies() {
        return new ArrayList<>(hobbies); // Return copy
    }
}
```

### Is `String` immutable in Java? Why?

Yes, `String` is immutable because:

* Its `char[]` value is marked `final`
* It enables **string pool** optimizations
* Prevents **security vulnerabilities** (e.g., in class loading, URLs)

### Why is immutability useful in multithreaded applications?

* Immutable objects are **inherently thread-safe**
* No need for synchronization or locking
* Prevents race conditions
* Ideal for **shared configuration** and **DTOs** in concurrent systems

### Can we have a class with some mutable and some immutable fields?

We **can**, but such a class is **not fully immutable**.
If one field can change, the object's state is considered mutable.
If absolutely needed, expose only unmodifiable or defensive copies.

### What is defensive copying?

**Defensive copying** is the practice of **copying a mutable object** when setting or returning it, to prevent changes to the internal state.

```java
this.hobbies = new ArrayList<>(hobbies); // In constructor  
return new ArrayList<>(this.hobbies);    // In getter
```

### Can we use `clone()` instead of defensive copying?

We **can**, but it's not recommended unless:

* The object implements `Cloneable` properly
* Deep cloning is required

Better alternatives: copy constructors, factory methods, or libraries like **Lombok**, **ModelMapper**.

### Can final make an object immutable?

**No.** `final` makes a **reference variable** unchangeable - not the object itself.

```java
final List<String> list = new ArrayList<>();
list.add("Java"); // Allowed! List itself is mutable
```

To make an object immutable, the **class design** must prevent mutation.

### How is immutability related to functional programming?

Functional programming promotes:

* Pure functions
* No side effects
* Stateless design

Immutability aligns with this by **ensuring objects can't be changed**, supporting **referential transparency** and safe **concurrency**.

## Method Overloading vs Overriding

### What is the difference between method overloading and method overriding?

| Feature          | Overloading                            | Overriding                              |
| ---------------- | -------------------------------------- | --------------------------------------- |
| Definition       | Same method name, different parameters | Same method name/signature, in subclass |
| Class Scope      | Same class or subclass                 | Subclass only                           |
| Return Type      | Can be different                       | Must be same (or covariant)             |
| Polymorphism     | Compile-time                           | Runtime                                 |
| Access Modifier  | No restriction                         | Can't reduce visibility                 |
| `static` Methods | Can be overloaded                      | Cannot be overridden                    |

### Can we overload a method by changing only the return type?

**No**. Return type alone is not enough to distinguish between overloaded methods.

```java
// Compilation error
int sum(int a, int b) { return a + b; }
double sum(int a, int b) { return a + b; }
```

### Can we override a private or static method?

* **Private methods** cannot be overridden - they are not inherited.
* **Static methods** are **hidden**, not overridden. This is called **method hiding**.

### What is method hiding in Java?

When a static method in subclass has the same signature as a static method in the superclass, it **hides** the superclass method - it doesn't override it.

```java
class A {
    static void show() { System.out.println("A"); }
}

class B extends A {
    static void show() { System.out.println("B"); }
}

A obj = new B();
obj.show(); // Output: A
```

### Can we override a method with a more restrictive access modifier?

**No**. We can't reduce the visibility of the overridden method.
For example, a `public` method in the superclass **cannot** be overridden with a `protected` or `private` method in subclass.

### What is covariant return type?

If a method in a subclass overrides a method in the superclass, it can return a **subtype** of the return type declared in the superclass.

```java
class A {
    Object getValue() { return null; }
}

class B extends A {
    @Override
    String getValue() { return "hello"; }
}
```

## Java Enums

### What is an `enum` in Java?

An `enum` (short for **enumeration**) is a special data type that enables a variable to be a set of predefined constants.

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY
}
```

### What are the benefits of using `enum`?


* Type-safe representation of constant values
* Can have **fields**, **constructors**, **methods**
* Prevents invalid values at compile time
* Readable and maintainable

### Can Java `enum` have fields and methods?

Yes. Java enums can have:

* Fields
* Constructors
* Methods (including abstract)
* We can even **override** methods in each constant

```java
enum Status {
    SUCCESS(200), ERROR(500);

    private final int code;
    Status(int code) { this.code = code; }
    public int getCode() { return code; }
}
```

### Can an `enum` implement an interface?

Yes. Enums can implement interfaces but **cannot extend classes** (because `enum` implicitly extends `java.lang.Enum`).

```java
interface Printable {
    void print();
}

enum Color implements Printable {
    RED, GREEN, BLUE;

    public void print() {
        System.out.println(this.name());
    }
}
```

### Can we override a method for each enum constant?

Yes. We can define different behavior per constant using **anonymous class body**:

```java
enum Operation {
    ADD {
        public int apply(int a, int b) { return a + b; }
    },
    MULTIPLY {
        public int apply(int a, int b) { return a * b; }
    };

    public abstract int apply(int a, int b);
}
```

### Can enums have constructors?

Yes, but they are **always private** (implicitly). We can't call them directly.

```java
enum Priority {
    HIGH(3), MEDIUM(2), LOW(1);

    private int level;
    Priority(int level) { this.level = level; }
    public int getLevel() { return level; }
}
```

### What is the difference between `==` and `.equals()` for enums?

Both `==` and `.equals()` compare enum constants by reference — they are **singleton instances**.

```java
Priority p1 = Priority.HIGH;
Priority p2 = Priority.HIGH;
System.out.println(p1 == p2);        // true
System.out.println(p1.equals(p2));   // true
```

### Can we use enums in switch statements?

Yes. Enums are frequently used with `switch`:

```java
switch (day) {
    case MONDAY: System.out.println("Start of week"); break;
    case FRIDAY: System.out.println("Weekend ahead!"); break;
}
```

### How do we get all values of an enum?

Use the built-in static `values()` method:

```java
for (Day d : Day.values()) {
    System.out.println(d);
}
```

### Can enums be compared using `compareTo()`?

Yes. Enums implement `Comparable` and are ordered by their **declaration position**:

```java
System.out.println(Day.MONDAY.compareTo(Day.WEDNESDAY)); // Output: -2
```

### Is `enum` thread-safe?

Yes. Enum constants are implicitly **singleton** and **thread-safe** by design.

### Can we use `enum` as a singleton?

Yes. This is a best practice since enum singletons are:

* **Serialization-safe**
* **Reflection-safe**

```java
enum MySingleton {
    INSTANCE;

    public void doSomething() {
        System.out.println("Singleton logic");
    }
}
```

### What's the base class of all enums?

All enums implicitly extend **`java.lang.Enum<E extends Enum<E>>`**.
This is why we can't extend another class.

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

## Comparator vs Comparable in Java

### What is the difference between `Comparable` and `Comparator` in Java?

| Feature             | `Comparable`                    | `Comparator`                 |
| ------------------- | ------------------------------- | ---------------------------- |
| Package             | `java.lang`                     | `java.util`                  |
| Purpose             | Natural ordering                | Custom ordering              |
| Interface Method    | `compareTo(T o)`                | `compare(T o1, T o2)`        |
| Implementation Site | In the class being sorted       | Separate class or lambda     |
| Modifies class?     | Yes (modifies original class) | No (keeps class untouched) |

### How does `Comparable` work in Java?

The class implements `Comparable<T>` and overrides the `compareTo()` method to define its **natural ordering**.

```java
public class Student implements Comparable<Student> {
    int id;
    String name;

    public int compareTo(Student other) {
        return this.id - other.id;
    }
}
```

### How does `Comparator` work in Java?

`Comparator` defines a **custom order** for objects. It can be passed to sorting methods like `Collections.sort()` or `List.sort()`.

```java
Comparator<Student> nameComparator = (s1, s2) -> s1.name.compareTo(s2.name);
Collections.sort(studentList, nameComparator);
```

### Can we use both `Comparable` and `Comparator` in the same class?

Yes. `Comparable` defines the **default (natural) sort order**, while `Comparator` can be used for **alternative sort orders**.

### How does Java handle sorting when both are present?

* If we use `Collections.sort(list)` -> it uses `Comparable`.
* If we use `Collections.sort(list, comparator)` -> it uses `Comparator`.

### Can we sort a list of objects without modifying the class?

Yes, by using `Comparator`. It keeps our original class **closed for modification** (follows Open-Closed Principle).

```java
List<Student> students = ...;
students.sort(Comparator.comparing(Student::getName));
```

### What happens if we don't implement `Comparable` and try to sort a list?

If we use `Collections.sort(list)` without a `Comparator`, and the class doesn't implement `Comparable`, it will throw:

```
java.lang.ClassCastException
```

### What is the return contract of `compareTo()` and `compare()` methods?

* Return **negative**: `this < other`
* Return **zero**: `this == other`
* Return **positive**: `this > other`

### What are Java 8+ ways to simplify Comparator logic?

```java
students.sort(Comparator.comparing(Student::getName));
students.sort(Comparator.comparingInt(Student::getId).reversed());
```

Java 8+ methods like `comparing()`, `thenComparing()`, `reversed()` greatly reduce boilerplate.

### How to sort using multiple fields (chaining comparators)?

```java
students.sort(
    Comparator.comparing(Student::getName)
              .thenComparingInt(Student::getId)
);
```

### Can we use lambda expressions to define `Comparator`?

Yes. Lambdas make defining comparators very concise.

```java
Comparator<Employee> comp = (e1, e2) -> e1.salary - e2.salary;
```

### What is the difference between `equals()` and `compareTo()`?

| Feature            | `equals()`                       | `compareTo()`                       |
| ------------------ | -------------------------------- | ----------------------------------- |
| Returns            | `boolean`                        | `int` (positive, zero, or negative) |
| Used in            | Logical equality (e.g., HashSet) | Sorting and ordering                |
| Override required? | Yes, for collections equality  | Yes, for natural sorting          |

### What if `compareTo()` returns inconsistent values with `equals()`?

Collections like `TreeSet`, `TreeMap` may behave incorrectly - such as treating equal objects as distinct.

**Best Practice:**
Ensure that `compareTo()` returns `0` **if and only if** `equals()` returns `true`.

### Can we write a Comparator to sort strings ignoring case?

```java
List<String> list = Arrays.asList("apple", "Banana", "cherry");
list.sort(String.CASE_INSENSITIVE_ORDER);
```

Or:

```java
list.sort((s1, s2) -> s1.compareToIgnoreCase(s2));
```

### Where are `Comparable` and `Comparator` used in Java libraries?

* `TreeSet`, `TreeMap`: rely on `compareTo()` or `Comparator`
* `Arrays.sort()`, `Collections.sort()`: use both
* Java Streams: `.sorted(Comparator)` for sorting

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

### How do we compose two `Function`s?

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

### Can we return a `Function` from a method?

Yes. Since functions are objects in Java 8, we can return them:

```java
public Function<String, String> getUppercaseFunction() {
    return str -> str.toUpperCase();
}
```

### Is `Function<T, R>` serializable?

No, it is **not serializable** by default. We can make it serializable by combining with the `Serializable` interface explicitly.

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

## Default & Static Methods in Interfaces (Java 8)

### What are default methods in interfaces? Why were they introduced?

A **default method** in an interface provides a **concrete implementation** so that implementing classes don’t have to override it.

**Introduced in Java 8** to enable **interface evolution** without breaking existing implementations.

```java
interface Vehicle {
    default void start() {
        System.out.println("Vehicle is starting");
    }
}
```

### How is a default method different from an abstract method in an interface?

| Feature             | Abstract Method | Default Method           |
| ------------------- | --------------- | ------------------------ |
| Must be implemented | Yes             | No (optional override)   |
| Contains body       | No              | Yes                      |
| Java version        | Since Java 1.0  | Since Java 8             |

### Can a class override a default method from an interface?

Yes, a class **can override** a default method if it wants a different behavior.

```java
class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car is starting");
    }
}
```

### What if a class implements two interfaces with the same default method?

We must **override** the conflicting method to resolve ambiguity.

```java
interface A {
    default void greet() { System.out.println("Hello from A"); }
}

interface B {
    default void greet() { System.out.println("Hello from B"); }
}

class C implements A, B {
    public void greet() {
        A.super.greet(); //or B.super.greet()
    }
}
```

### What are static methods in interfaces?

Java 8 introduced **static methods in interfaces** that can be called **only on the interface**, not on implementing classes.

```java
interface Utils {
    static void log(String msg) {
        System.out.println("Log: " + msg);
    }
}
```

**Usage:**

```java
Utils.log("test");
```

### Can default or static methods be overridden in implementing classes?

| Method Type | Can be Overridden?           |
| ----------- | ---------------------------- |
| Default     | Yes, in implementing class |
| Static      | No, not inherited at all   |

### Why do static methods in interfaces not participate in inheritance?

To avoid **name clashes** and confusion. Static methods are **utility methods** tied to the interface - not to the implementing class.

### Can interfaces have private methods in Java 8?

No. **Private methods in interfaces were introduced in Java 9**, not Java 8.

### What is the purpose of default methods in interfaces?

To allow **backward-compatible interface evolution** - adding new methods without breaking existing implementations.

### Can default methods access static or private methods in the interface?

* Default methods can call **static methods**.
* They **cannot** access **private methods** in Java 8.

```java
default void logStart() {
    Utils.log("Starting...");
}
```

### Can abstract classes have default methods like interfaces?

No concept of **"default" keyword** in abstract classes. But **abstract classes can have concrete methods**, which serve a similar purpose.

### What are the limitations of default methods in Java 8?

* Can't override `Object` methods like `equals()`, `hashCode()`, etc.
* Can't be `synchronized`, `native`, or `strictfp`
* Can't define instance fields
* Only one implementation per interface (no multiple versions)

### How are default methods implemented internally by the JVM?

They're treated as **virtual methods** in the interface and invoked using `invokeinterface` bytecode instruction. They are stored in the interface's constant pool but dispatched **dynamically** at runtime.

### Give an example of a default method used in Java 8 APIs.

```java
List<String> names = Arrays.asList("Aman", "Shreya");
names.forEach(System.out::println); //forEach is a default method in Iterable
```

### How do default methods relate to functional interfaces?

Functional interfaces can have **default and static methods**, but still must have **only one abstract method**.

```java
@FunctionalInterface
interface Converter<T, R> {
    R convert(T input);

    default void log(T input) {
        System.out.println("Converting " + input);
    }
}
```

## JVM Internals

### JVM Memory Structure

```
         ______________________________
        |         Metaspace           | <- (off-heap, class metadata)
        |_____________________________|
        |        Old Generation       | <- Long-lived objects
        |_____________________________|
        |        Young Generation     | <- New objects (Eden + Survivor)
        |_____________________________|
        |_____________________________|
        |        Java Stack           | <- One per thread (method frames)
        |_____________________________|
```

### What is the JVM? How is it different from JRE and JDK?

* **JVM (Java Virtual Machine):** Executes Java bytecode and handles memory, garbage collection, and runtime operations.
* **JRE (Java Runtime Environment):** JVM + core libraries; required to run Java programs.
* **JDK (Java Development Kit):** JRE + development tools (javac, javadoc); required to write and compile Java programs.

### What are the key memory areas allocated by the JVM?

| Memory Area              | Description                                               |
| ------------------------ | --------------------------------------------------------- |
| **Heap**                 | Stores objects and class instances                        |
| **Stack**                | Stores method call frames, local variables, return values |
| **Method Area**          | Stores class metadata, method definitions, constants      |
| **Program Counter (PC)** | Stores current execution address of each thread           |
| **Native Method Stack**  | Used for native (non-Java) methods                        |

### What is the difference between Heap and Stack memory in JVM?

| Feature     | Heap                     | Stack                          |
| ----------- | ------------------------ | ------------------------------ |
| Stores      | Objects, class instances | Method frames, local variables |
| Scope       | Shared across threads    | Thread-specific                |
| Size        | Larger                   | Smaller                        |
| GC eligible | Yes                    | No (automatically popped)    |

### What is the Method Area (or Metaspace)?

The Method Area (now **Metaspace in Java 8**) stores:

* Class metadata (name, fields, methods)
* Static variables
* Constant pool

In Java 8+, Metaspace is **off-heap** and grows dynamically by default.

### What is the ClassLoader in Java?

A **ClassLoader** is responsible for loading class bytecode into memory when the class is referenced for the first time.

**Types of ClassLoaders:**

* **Bootstrap ClassLoader**: Loads core Java classes (rt.jar)
* **Extension ClassLoader**: Loads classes from `ext` directory
* **Application ClassLoader**: Loads classes from the application classpath
* **Custom ClassLoader**: User-defined for specific use cases

### What is the class loading process in JVM?

1. **Loading** - Loads `.class` bytecode into memory
2. **Linking**:

   * **Verification** - Checks bytecode integrity
   * **Preparation** - Allocates memory for static variables
   * **Resolution** - Resolves symbolic references to actual memory locations
3. **Initialization** - Executes static blocks and initializes static fields

### What is a ClassLoader leak?

A **ClassLoader leak** happens when classes or resources aren't properly unloaded, usually in **web applications** (e.g., re-deployments in Tomcat), causing memory to grow uncontrollably.

### What are the main Garbage Collection (GC) generations in JVM?

* **Young Generation**:

  * Eden + 2 Survivor spaces
  * New objects are allocated here
  * Collected using **Minor GC**

* **Old (Tenured) Generation**:

  * Long-lived objects promoted from Young Gen
  * Collected using **Major/Full GC**

* **Metaspace** (Java 8+):

  * Stores class metadata (replaces PermGen)

### What is Minor GC vs Major GC?

| GC Type       | Applies To       | Frequency  | Pause Time |
| ------------- | ---------------- | ---------- | ---------- |
| Minor GC      | Young Generation | Frequent   | Short      |
| Major/Full GC | Old Generation   | Less often | Long pause |

### What are some common JVM tuning flags (basic level)?

```bash
-Xms512m                  # Initial heap size
-Xmx1024m                 # Maximum heap size
-XX:MetaspaceSize=128m    # Initial Metaspace size (Java 8+)
-XX:MaxMetaspaceSize=512m # Max Metaspace size
-XX:+UseG1GC              # Use G1 garbage collector
-XX:+PrintGCDetails       # Print GC logs
```

### What happens when the heap memory is full?

If the GC cannot reclaim enough space, JVM throws:

```bash
java.lang.OutOfMemoryError: Java heap space
```

### What is JIT (Just-In-Time) Compilation in JVM?

JIT compiles frequently used bytecode to **native machine code at runtime**, improving performance by avoiding repeated interpretation.

### What is the role of the Program Counter Register in JVM?

Each thread has a **Program Counter (PC)** register that points to the **current instruction** being executed. It is thread-specific and helps support **multi-threading**.

## Java Predicate

### What is a `Predicate` in Java?

A `Predicate<T>` is a **functional interface** introduced in Java 8 (in `java.util.function` package) that takes an input of type `T` and returns a `boolean` result. It's commonly used for filtering, matching, and testing conditions in a **functional style**.

### What is the method signature of the `Predicate` interface?

```java
boolean test(T t);
```

This method evaluates a condition (predicate) on the given argument and returns `true` or `false`.

### How do we use `Predicate` with Java Streams?

We typically use it with the `filter()` method:

```java
List<String> names = List.of("Aman", "Harsh", "Anil");
Predicate<String> startsWithA = name -> name.startsWith("A");

List<String> filtered = names.stream()
    .filter(startsWithA)
    .collect(Collectors.toList());
```

**Output:** `[Aman, Anil]`

### Can we combine two predicates? How?

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

### Can we pass a `Predicate` to a method?

Yes. Since it's a functional interface, we can pass it as a lambda or method reference.

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

### Can we chain multiple `Predicate`s in a single stream?

Yes. Example:

```java
Predicate<String> startsWithA = s -> s.startsWith("A");
Predicate<String> lengthIs5 = s -> s.length() == 5;

List<String> filtered = names.stream()
    .filter(startsWithA.and(lengthIs5))
    .collect(Collectors.toList());
```

### Is `Predicate` serializable?

No, `Predicate` is **not serializable** by default. If we need serialization, we need to implement a custom `Predicate` that extends both `Predicate` and `Serializable`.

### How would we write a custom reusable predicate?

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

No. Once a terminal operation is called, the stream is **consumed** and cannot be reused. We must create a new stream.

### How do we count the number of strings in a list that start with a specific letter (e.g., "A")?

```java
List<String> names = List.of("Aman", "Bob", "Adam", "Ankit", "Brian");

long count = names.stream()
                  .filter(name -> name.startsWith("A"))
                  .count();

System.out.println(count); // Output: 3
```

### How do we find the maximum number in a list using Stream?

```java
List<Integer> numbers = List.of(10, 25, 3, 47, 15);

int max = numbers.stream()
                 .max(Integer::compare)
                 .orElseThrow();

System.out.println(max); // Output: 47
```

### How do we convert a list of strings to uppercase using Stream API?

```java
List<String> names = List.of("Akshay", "doe", "Aman");

List<String> upper = names.stream()
                          .map(String::toUpperCase)
                          .collect(Collectors.toList());

System.out.println(upper); // [Akshay, DOE, Aman]
```

### How do we remove duplicates from a list using Stream?

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

### How do we join a list of strings with commas using Stream API?

```java
List<String> words = List.of("Java", "Python", "Go");

String result = words.stream()
                     .collect(Collectors.joining(", "));

System.out.println(result); // Output: Java, Python, Go
```

### How do we calculate the sum of all even numbers in a list?

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

int sum = numbers.stream()
                 .filter(n -> n % 2 == 0)
                 .mapToInt(Integer::intValue)
                 .sum();

System.out.println(sum); // Output: 12
```

### How do we sort a list of strings in reverse alphabetical order?

```java
List<String> names = List.of("Apple", "Mango", "Banana");

List<String> sorted = names.stream()
                           .sorted(Comparator.reverseOrder())
                           .collect(Collectors.toList());

System.out.println(sorted); // [Mango, Banana, Apple]
```

### How do we filter a list of objects based on a field?

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
    new Employee("Akshay", 4000),
    new Employee("Akash", 6000),
    new Employee("Mark", 7000)
);

List<Employee> highEarners = employees.stream()
    .filter(e -> e.salary > 5000)
    .collect(Collectors.toList());
```

### How do we count the frequency of elements in a list?

```java
List<String> items = List.of("apple", "banana", "apple", "orange", "banana", "apple");

Map<String, Long> freq = items.stream()
    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

System.out.println(freq);
// Output: {orange=1, banana=2, apple=3}
```

### How do we partition a list into two based on a condition?

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
    new Employee("Akshay", "IT"),
    new Employee("Akash", "HR"),
    new Employee("Aman", "IT")
);

Map<String, List<Employee>> grouped = employees.stream()
    .collect(Collectors.groupingBy(emp -> emp.dept));

grouped.forEach((k, v) -> System.out.println(k + " => " + v.stream().map(e -> e.name).toList()));
// Output:
// IT => [Akshay, Aman]
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

### Group employees by the first character of their name and sort the map's keys in descending order (Z to A) and finally sort the employees in each group in ascending order by name

```java
public class Main {
    public static void main(String[] args) {
        List<Employees> employees = Arrays.asList(new Employees("Aman", 30, 70000), new Employees("Bobby", 45, 90000),
				new Employees("Cherry", 35, 66000), new Employees("David", 38, 64000),
				new Employees("Janet", 28, 80000), new Employees("David", 28, 90000), new Employees("Aman", 32, 72000));

		Map<Character, List<Employees>> emp = employees.stream()
				.collect(Collectors.groupingBy(e -> Character.toUpperCase(e.getName().charAt(0)),
						() -> new TreeMap<>(Comparator.reverseOrder()),
						Collectors.collectingAndThen(Collectors.toList(), list -> list.stream()
								.sorted(Comparator.comparing(Employees::getName)).collect(Collectors.toList()))));
    }
}

class Employees {
	private String name;
	private int age;
	private double salary;

	public Employees() {
		super();
	}

	public Employees(String name, int age, double salary) {
		super();
		this.name = name;
		this.age = age;
		this.salary = salary;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public double getSalary() {
		return salary;
	}

	public void setSalary(double salary) {
		this.salary = salary;
	}

	@Override
	public String toString() {
		return "Employees [name=" + name + ", age=" + age + ", salary=" + salary + "]";
	}
}
```

## Java 8 Method References & Lambda Expressions

### Lambda Expressions

### What is a lambda expression in Java?

A lambda expression is a **short block of code** which takes in parameters and returns a value.
It's used to implement **functional interfaces** (interfaces with a single abstract method).

**Syntax:**

```java
(parameter) -> expression
```

**Example:**

```java
Comparator<Integer> comp = (a, b) -> Integer.compare(a, b);
```

### What is the benefit of using lambdas?

* More concise and readable code
* Enables **functional programming**
* Supports **functional chaining**
* Eliminates boilerplate of anonymous classes

### What are functional interfaces?

An interface with exactly **one abstract method** is a functional interface.
They can have default or static methods.

Examples:

* `Runnable`, `Comparator<T>`, `Callable<V>`
* Java 8 interfaces: `Predicate<T>`, `Function<T, R>`, `Supplier<T>`, `Consumer<T>`

Annotation:

```java
@FunctionalInterface
public interface MyInterface {
    void doSomething();
}
```

### What is the syntax difference between anonymous classes and lambdas?

**Anonymous class:**

```java
Runnable r = new Runnable() {
    public void run() {
        System.out.println("Running");
    }
};
```

**Lambda:**

```java
Runnable r = () -> System.out.println("Running");
```

Lambdas are more concise and readable.

### Can lambdas access variables from the enclosing scope?

Yes, but only **effectively final** variables — i.e., variables that are not changed after assignment.

```java
int factor = 2;
Function<Integer, Integer> multiplier = x -> x * factor;
```

### What is functional chaining with lambdas?

**Functional chaining** is the ability to **chain multiple operations** (map, filter, reduce) using lambdas.

**Example:**

```java
List<String> names = Arrays.asList("Tom", "Jerry", "Bob");
names.stream()
     .filter(s -> s.length() > 3)
     .map(String::toUpperCase)
     .forEach(System.out::println);
```

This makes code more **declarative**, **clean**, and **pipeline-friendly**.

### Method References

### What is a method reference?

Method reference is a **shorthand** for calling a method via a lambda, when the lambda **only calls an existing method**.

**Syntax:**

```java
ClassName::methodName
```

### What are the types of method references?

| Type                       | Syntax Example              |
| -------------------------- | --------------------------- |
| Static method              | `ClassName::staticMethod`   |
| Instance method (specific) | `instance::instanceMethod`  |
| Instance method (any)      | `ClassName::instanceMethod` |
| Constructor reference      | `ClassName::new`            |

### How do method references improve readability?

They reduce boilerplate by **removing repetitive code** that merely forwards arguments to another method.

**Example:**

```java
list.forEach(item -> System.out.println(item)); //Lambda
list.forEach(System.out::println);              //Method reference
```

### Can method references be used with functional interfaces?

Yes. Wherever a lambda can be used, a method reference can be used **if the signature matches**.

```java
Function<String, Integer> parse = Integer::parseInt;
```

### What will this return?

```java
Function<String, String> trim = String::trim;
System.out.println(trim.apply("  Hello  "));
```

Output: `"Hello"`
The method reference `String::trim` binds the method `trim()` of any `String` instance.

### When would we choose a lambda over a method reference?

Use a **lambda** when:

* We need additional logic
* We're composing multiple operations
* Method reference doesn't match our intent clearly

Use a **method reference** when:

* The lambda **just calls an existing method**
* It improves **conciseness and clarity**

### What does this code do?

```java
List<String> names = Arrays.asList("Anna", "Mike", "Bob");
names.stream()
     .sorted(String::compareToIgnoreCase)
     .forEach(System.out::println);
```

It sorts the names alphabetically (case-insensitive) and prints them.

### What is the return type of a lambda?

The return type is **inferred** by the compiler from the context of the **functional interface** being implemented.

### Can lambdas throw exceptions?

Yes, but only **unchecked exceptions**.
For checked exceptions, the functional interface must declare `throws` clause explicitly (e.g., `Callable<T>`).

## Exceptions in Java

### What is the difference between checked and unchecked exceptions?

* **Checked exceptions**: Subclass of `Exception` (excluding `RuntimeException`), must be declared using `throws` or handled in a `try-catch`. Example: `IOException`, `SQLException`.
* **Unchecked exceptions**: Subclass of `RuntimeException`, not checked at compile time. Example: `NullPointerException`, `IndexOutOfBoundsException`.

### Can we create a custom exception in Java?
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

* We want to **ensure resource cleanup**, regardless of whether an exception occurs or not.
* We're handling exceptions elsewhere (e.g., with `throws`) but still want to release resources.

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

### Can we override a `finalize()` method?
Yes, but it's **not recommended**. Also, it's deprecated starting from Java 9 and removed in later versions. Use `java.lang.ref.Cleaner` or try-with-resources instead.

## How `hashCode()` and `equals()` are linked in Java

### Why should we override `hashCode()` when overriding `equals()`?
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

## Java Serialization

### What is serialization in Java?

Serialization is the process of converting an object into a **byte stream** so it can be:

* Stored in a file or database
* Sent over a network
  Later, it can be reconstructed using **deserialization**.

### Which interface must a class implement to support serialization?

The class must implement the **`java.io.Serializable`** marker interface.

```java
public class User implements Serializable {
    private String name;
    private int age;
}
```

It has **no methods** - it's a **marker interface**.

### What is `serialVersionUID`?

`serialVersionUID` is a **unique identifier** used during deserialization to verify that the sender and receiver of a serialized object have compatible class definitions.

```java
private static final long serialVersionUID = 1L;
```

If not declared, Java generates one at runtime - but this may change with class modifications, leading to `InvalidClassException`.

### What happens if a class does not declare `serialVersionUID`?

Java will generate one at runtime based on class structure.
If the class changes (e.g., add/remove field), the generated ID may change, breaking compatibility during deserialization.

Best practice: **Always declare it manually**.

### What is the difference between `Serializable` and `Externalizable`?

| Feature        | `Serializable`   | `Externalizable`                        |
| -------------- | ---------------- | --------------------------------------- |
| Interface Type | Marker interface | Has methods to implement                |
| Custom Control | No               | Yes (`writeExternal`, `readExternal`)   |
| Performance    | Slower           | Faster (if used correctly)              |

### What is the role of the `transient` keyword?

Fields marked with `transient` are **excluded from serialization**.

```java
transient String password;
```

Useful for:

* Sensitive data
* Temporary/cache fields
* Objects that are not serializable

### What is the default serialization mechanism?

Java uses **reflection** to serialize object state (all non-transient, non-static fields). It includes:

* Class name
* Field values
* Object graph (recursively)

### What is `ObjectOutputStream` and `ObjectInputStream`?

* `ObjectOutputStream`: Serializes objects to an output stream
* `ObjectInputStream`: Deserializes objects from an input stream

Example:

```java
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("data.ser"));
out.writeObject(myObject);

ObjectInputStream in = new ObjectInputStream(new FileInputStream("data.ser"));
MyClass obj = (MyClass) in.readObject();
```

### What happens if a serialized object contains a reference to another object?

If the referenced object is **also Serializable**, it will be serialized recursively.
If not, `NotSerializableException` is thrown.

### Can we serialize static fields?

**No.** Static fields belong to the class, not the object instance, so they are not serialized.

### Can a superclass be non-serializable while the subclass is serializable?

**Yes**, but during deserialization, the non-serializable superclass must have a **no-arg constructor**, as it will be called to reinitialize its fields.

### What is `readObject()` and `writeObject()`?

Special private methods that can be added to control the default serialization logic:

```java
private void writeObject(ObjectOutputStream oos) throws IOException {
    oos.defaultWriteObject();
    // custom logic
}

private void readObject(ObjectInputStream ois) throws IOException, ClassNotFoundException {
    ois.defaultReadObject();
    // custom logic
}
```

### What are common exceptions in serialization?

| Exception                  | Cause                                          |
| -------------------------- | ---------------------------------------------- |
| `NotSerializableException` | Class or one of its fields is not serializable |
| `InvalidClassException`    | `serialVersionUID` mismatch                    |
| `IOException`              | I/O failure during stream operations           |

### How to make a singleton class serializable and preserve singleton property?

Implement `readResolve()` method:

```java
protected Object readResolve() {
    return getInstance(); // return the existing singleton instance
}
```

## Multithreading in Java

### What is the difference between `Runnable` and `Callable`?

* `Runnable`: Does not return a result, cannot throw checked exceptions.
* `Callable<V>`: Returns a result (`V`) and can throw checked exceptions. Used with `ExecutorService`.

### How do we prevent thread-safety issues in Java?

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

## Java Generics

### What are Generics in Java?

Generics allow classes, interfaces, and methods to operate on **typed parameters** - enabling **compile-time type safety** and eliminating the need for explicit typecasting.

```java
List<String> list = new ArrayList<>();
```

Without generics, this would require casting:

```java
List list = new ArrayList();
String name = (String) list.get(0); //unsafe
```

### What are the benefits of using generics?

* Compile-time type safety
* Eliminate unnecessary casting
* Reusability of code (generic algorithms)
* Better readability and maintainability

### What is type erasure in Java?

Type erasure is the process by which **generic type information is removed at runtime**. This is done to ensure **backward compatibility** with older versions of Java.

For example:

```java
List<String> list = new ArrayList<>();
```

At runtime, it's treated as:

```java
List list = new ArrayList(); //Raw type
```

**Implication:**

* No method overloading based on generic types.
* We cannot check `instanceof` with a generic type (`if (obj instanceof List<String>)`).

### Can we overload a method using generic types?

**No.** Due to **type erasure**, the following methods will cause a compilation error:

```java
public void process(List<String> list) { }
public void process(List<Integer> list) { } //Not allowed
```

---

### What are wildcards in generics?

Wildcards (`?`) represent an **unknown type** in generics. Used to make methods **more flexible**.

```java
List<?> unknownList = new ArrayList<String>();
```

Wildcards are often used with `extends` (upper bound) or `super` (lower bound).

### What is bounded wildcard (`extends`) in Java Generics?

It restricts the unknown type to be a **subclass of a specific class** (upper bound).

```java
public void printList(List<? extends Number> list)
```

Allows: `List<Integer>`, `List<Double>`, etc.

**Used when we only read values** (covariant behavior).

### What is lower-bounded wildcard (`super`) in Java Generics?

It restricts the unknown type to be a **superclass** of a specific type.

```java
public void addToList(List<? super Integer> list)
```

Allows: `List<Integer>`, `List<Number>`, `List<Object>`

**Used when we want to write values into a structure** (contravariant behavior).

### What is the PECS principle in Java Generics?

**PECS** stands for:

> **Producer Extends, Consumer Super**

* Use `? extends T` when a structure **produces** T (for reading).
* Use `? super T` when a structure **consumes** T (for writing).

| Operation | Wildcard      | Example                  |
| --------- | ------------- | ------------------------ |
| Read      | `? extends T` | `List<? extends Number>` |
| Write     | `? super T`   | `List<? super Integer>`  |

### What is an unbounded wildcard in Java?

`List<?>` is an unbounded wildcard, meaning it can refer to a list of **any type**.

Useful for read-only access when type is not relevant.

### Can we use primitive types with generics?

No. Java generics work only with **reference types**, not primitives.

We must use wrapper classes:

```java
List<int>     //Invalid
List<Integer> //Valid
```

### Can we create arrays of generic types?

No. Generic arrays are not allowed because of **type erasure**.

```java
List<String>[] arr = new List<String>[10]; //Compilation error
```

Alternative: use `List<?>[]` with care or collections instead.

### What is the difference between `List<Object>` and `List<?>`?

| Feature             | `List<Object>`            | `List<?>`                  |
| ------------------- | ------------------------- | -------------------------- |
| Accepts all types?  | Only objects explicitly | Any generic list         |
| Can write elements? | Yes                     | No (only read or `null`) |

### What are raw types in Java?

Raw types are generic types **without type parameters**.
E.g., `List list = new ArrayList();`

Allowed for **backward compatibility**, but not type-safe - leads to runtime `ClassCastException`.

### Can we make a generic class or method in Java?

Yes.

**Generic class:**

```java
class Box<T> {
    private T value;
    public void set(T value) { this.value = value; }
    public T get() { return value; }
}
```

**Generic method:**

```java
public <T> void print(T item) {
    System.out.println(item);
}
```

### How do we restrict a generic class to accept only certain types?

Use bounded types:

```java
class Calculator<T extends Number> {
    T value;
}
```

Now `T` can only be `Integer`, `Double`, etc. - not `String`.

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

## Java 8 Memory Management

### What major memory management change was introduced in Java 8?

The biggest change was the **removal of PermGen (Permanent Generation)** and the introduction of a new memory area called **Metaspace**.

### What was PermGen and why was it removed?

**PermGen** (Permanent Generation) was a part of the heap used to store:

* Class metadata (class names, methods, constant pool)
* Static fields
* Interned strings (pre Java 7)

**Problems with PermGen:**

* Fixed in size; could cause `OutOfMemoryError: PermGen space`
* Hard to tune dynamically
* Non-uniform across platforms

Hence, it was removed and replaced with **Metaspace** in Java 8.

### What is Metaspace in Java 8?

**Metaspace** is a **native memory area (off-heap)** introduced in Java 8 that stores **class metadata**. Unlike PermGen, it **grows automatically** by default, depending on the system memory.

### How is Metaspace different from PermGen?

| Feature          | PermGen                           | Metaspace (Java 8+)                         |
| ---------------- | --------------------------------- | ------------------------------------------- |
| Location         | JVM Heap                          | Native Memory (off-heap)                    |
| Memory tuning    | `-XX:PermSize`, `-XX:MaxPermSize` | `-XX:MetaspaceSize`, `-XX:MaxMetaspaceSize` |
| Flexibility      | Fixed size                        | Dynamically grows                           |
| OOM risk         | Frequent `OutOfMemoryError`       | Less frequent (can still happen)            |
| Metadata storage | In heap                           | Outside heap                                |

### How can we limit the size of Metaspace in Java 8?

```bash
-XX:MetaspaceSize=256M
-XX:MaxMetaspaceSize=512M
```

* `MetaspaceSize`: Initial size (triggers first GC)
* `MaxMetaspaceSize`: Upper limit (default is unlimited)

### What happens when Metaspace runs out of memory?

WE get an **`OutOfMemoryError: Metaspace`**
It occurs if class metadata usage exceeds `MaxMetaspaceSize`.

### What is stored in Metaspace?

* Class metadata
* Method and field names
* Method bytecode
* Annotations
* Constant pools
* Reflection data

**Instance fields and static variables** are still stored on the Java heap.

### How can Metaspace leak happen in Java 8?

Typically occurs with **dynamic class loading**, especially in:

* Application servers (Tomcat, JBoss)
* Hot deployment / ClassLoader leaks

If classes are repeatedly loaded but never unloaded, Metaspace memory keeps growing.

### Does Java 8 move interned strings out of PermGen?

Yes. Starting from **Java 7**, interned strings were moved from **PermGen to the heap**.
In Java 8, since **PermGen is removed**, interned strings are stored on the **heap**.

### Did Java 8 introduce any other memory improvements?

Yes, other JVM-level performance enhancements in Java 8 include:

* **Compressed Class Pointers (CompressedClassSpaceSize)**
* Improved **Garbage Collectors** (e.g., G1 becoming more stable)
* Optimizations for **lambdas** and **method references**

---

### What are some tuning options for Metaspace?

```bash
-XX:MetaspaceSize=128M              #Initial size
-XX:MaxMetaspaceSize=512M           #Upper bound
-XX:CompressedClassSpaceSize=128M   #Compressed class pointers
```

### Is there still any GC overhead with Metaspace?

Yes, Metaspace growth may **trigger GC** events when reaching thresholds or needing cleanup of unused class metadata. However, tuning is easier compared to PermGen.

### How can we simulate a Metaspace OOM?

By loading too many classes dynamically via custom ClassLoaders in a loop:

```java
while (true) {
    URLClassLoader loader = new URLClassLoader(...);
    Class<?> clazz = loader.loadClass("SomeClass");
}
```

This keeps loading classes but never unloads them.

### Is Metaspace GC-ed like the heap?

Metaspace itself is **not garbage collected**, but the **class metadata** stored within it **can be reclaimed** when the associated ClassLoader is garbage collected.

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

When we want to define a family of algorithms, encapsulate each one, and make them interchangeable. Used for decoupling algorithm from the object using it.

### What is the difference between Factory and Abstract Factory Pattern?

* Factory: Creates objects without exposing the creation logic.
* Abstract Factory: Creates families of related or dependent objects without specifying their concrete classes.

### Singleton Pattern

### What is the Singleton Design Pattern?

The **Singleton Pattern** ensures that a class has **only one instance** and provides a **global access point** to it.

### How do we implement a thread-safe Singleton in Java?

**Using `synchronized` block (lazy initialization):**

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null)
            instance = new Singleton();
        return instance;
    }
}
```

**Using Bill Pugh (recommended):**

```java
public class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```

**Using enum (best way):**

```java
public enum SingletonEnum {
    INSTANCE;

    public void doSomething() { }
}
```

### Why is enum the best way to implement a Singleton?

* Enum is **thread-safe**
* Protects against **serialization**, **reflection**, and **cloning attacks**
* Simplest implementation

### How do we prevent reflection in Singleton?

Throw an exception if the constructor is called more than once:

```java
private Singleton() {
    if (instance != null)
        throw new RuntimeException("Use getInstance()");
}
```

### Factory Pattern

### What is the Factory Pattern?

The **Factory Pattern** defines an interface for creating an object but lets subclasses decide which class to instantiate.

Useful when the object creation logic is **complex or varies**.

### Give an example of the Factory Pattern in Java.

```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() { System.out.println("Circle"); }
}

class Rectangle implements Shape {
    public void draw() { System.out.println("Rectangle"); }
}

class ShapeFactory {
    public static Shape getShape(String type) {
        switch (type) {
            case "CIRCLE": return new Circle();
            case "RECTANGLE": return new Rectangle();
            default: throw new IllegalArgumentException("Unknown shape");
        }
    }
}
```

**Usage:**

```java
Shape shape = ShapeFactory.getShape("CIRCLE");
shape.draw();
```

### How is Factory Pattern different from Abstract Factory Pattern?

| Feature      | Factory Pattern | Abstract Factory Pattern          |
| ------------ | --------------- | --------------------------------- |
| Creates      | One product     | Related products families         |
| Pattern type | Creational      | Creational                        |
| Example      | `ShapeFactory`  | `UIFactory` (Windows/Mac Buttons) |

### Strategy Pattern

### What is the Strategy Pattern?

Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable at runtime.

### Give an example of Strategy Pattern in Java.

```java
interface PaymentStrategy {
    void pay(int amount);
}

class CreditCardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " via Credit Card");
    }
}

class PayPalPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " via PayPal");
    }
}

class ShoppingCart {
    private PaymentStrategy strategy;

    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void checkout(int amount) {
        strategy.pay(amount);
    }
}
```

**Usage:**

```java
ShoppingCart cart = new ShoppingCart();
cart.setPaymentStrategy(new PayPalPayment());
cart.checkout(100);
```

### Where is Strategy Pattern used in Java libraries?

* `Comparator<T>` in `Collections.sort(list, comparator)`
* `Runnable` interface in threads
* Spring: `BeanNameViewResolver`, `ResourceLoader`, `AuthenticationProvider`

### Observer Pattern

### What is the Observer Pattern?

Observer Pattern defines a **one-to-many relationship** between objects so that when one object changes state, all its dependents are notified.

### Give an example of Observer Pattern in Java.

```java
interface Observer {
    void update(String message);
}

class EmailSubscriber implements Observer {
    public void update(String message) {
        System.out.println("Email received: " + message);
    }
}

class MessagePublisher {
    private List<Observer> observers = new ArrayList<>();

    public void subscribe(Observer o) { observers.add(o); }
    public void unsubscribe(Observer o) { observers.remove(o); }

    public void notifyObservers(String message) {
        for (Observer o : observers) {
            o.update(message);
        }
    }
}
```

**Usage:**

```java
MessagePublisher publisher = new MessagePublisher();
Observer user1 = new EmailSubscriber();
publisher.subscribe(user1);
publisher.notifyObservers("New article published!");
```

### Where is Observer Pattern used in Java?

* `java.util.Observer` and `Observable` (deprecated in Java 9)
* Event systems like `ActionListener` in Swing
* Spring: `ApplicationEventPublisher`, `@EventListener`

### What are differences between Strategy and Observer?

| Feature      | Strategy                    | Observer                          |
| ------------ | --------------------------- | --------------------------------- |
| Pattern Type | Behavioral                  | Behavioral                        |
| Purpose      | Choose algorithm at runtime | Notify dependents of state change |
| Relationship | 1-to-1                      | 1-to-many                         |
| Example      | Payment strategies          | Event listener system             |

### Why are design patterns important in real-world projects?

* Provide proven solutions to common problems
* Improve **code reusability** and **flexibility**
* Promote **SOLID** principles
* Enhance **communication** among developers using common terminology

## Hashing, `equals()` and `hashCode()`

### What is hashing in Java?

Hashing is the process of converting an object (or key) into an integer (called **hash code**) that represents the bucket index where the object will be stored in **hash-based collections** like `HashMap` and `HashSet`.

### What is `hashCode()` in Java?

`hashCode()` is a method defined in `Object` class that returns an `int` hash value.
Collections like `HashMap`, `HashSet`, and `Hashtable` use it to **compute bucket location**.

### What is `equals()` in Java?

The `equals()` method compares two objects for **logical equality**.
We should override `equals()` when we want to define **custom object comparison** logic.

### What is the contract between `equals()` and `hashCode()`?

* If two objects are **equal** via `equals()`, they **must** return the **same** `hashCode()`.
* If two objects return **different** hash codes, they **must not** be equal.
* **If only `equals()` is overridden and not `hashCode()`**, hash-based collections may behave incorrectly.

### How does `HashMap` work internally?

1. Computes the **hash code** of the key using `hashCode()`.
2. Applies a hashing function to find the **bucket index**.
3. If bucket is **empty**, the entry is added.
4. If not, uses `equals()` to check if the key already exists:

   * If yes, it **updates** the value.
   * If no, it handles **collision** (chaining or tree-based node).

### How does `HashSet` use `hashCode()` and `equals()`?

`HashSet` is backed by a `HashMap`. It stores keys in the map with a dummy value.
So it uses `hashCode()` to find the bucket and `equals()` to check if the element already exists.

### Why should we override both `equals()` and `hashCode()` when using custom objects in a `HashSet` or `HashMap`?

If only `equals()` is overridden, two equal objects may **end up in different buckets**, causing **duplicate entries** or **lookup failures**.

### What happens if two keys have the same hash code in a `HashMap`?

* Hash collision occurs
* Java uses **separate chaining** (linked list or tree)
* It traverses the list and uses `equals()` to find the correct key

### What is the default bucket threshold for converting a collision list to a tree in `HashMap`?

If the number of items in a bucket exceeds **8**, and the map has more than **64 buckets**, it converts the list to a **balanced tree (TreeNode)** to improve lookup speed from O(n) -> O(log n).

### What is the default load factor of a `HashMap`?

`0.75` - when the number of entries exceeds **75% of the bucket size**, the capacity is **doubled** (resize operation).

### Sample: How would we override `equals()` and `hashCode()` in a custom class?

```java
public class Employee {
    private int id;
    private String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Employee)) return false;
        Employee emp = (Employee) o;
        return id == emp.id && name.equals(emp.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}
```

### Can `hashCode()` return negative values?

Yes, `hashCode()` can be negative. It returns a 32-bit integer.
However, collections internally use **`(hash & (n - 1))`** to calculate non-negative bucket index.

### What is a good `hashCode()` implementation?

* Should distribute values **evenly** across buckets
* Should be **consistent**: same object always returns same hash
* Should combine fields using a **prime number multiplier** (e.g., 31)

### Why is `Objects.hash(...)` recommended?

It simplifies writing `hashCode()` by safely handling `null` values and combining multiple fields efficiently.

```java
@Override
public int hashCode() {
    return Objects.hash(id, name, dept);
}
```

### Can we store `null` as a key or value in `HashMap` or `HashSet`?

* `HashMap` allows **1 null key** and **multiple null values**
* `HashSet` allows **1 null element** (because it's backed by a `HashMap`)

## Miscellaneous

### What is varargs in Java? When would we use it?

`varargs` allows a method to accept **zero or more arguments** of a specified type.
Syntax:

```java
public void printNames(String... names) {
    for (String name : names) {
        System.out.println(name);
    }
}
```

We use `varargs` when we don't know in advance how many parameters a method will receive.

### Can we have multiple varargs in a method signature?

**No**, a method can have **only one varargs parameter**, and it **must be the last parameter** in the method signature.

```java
// Invalid: varargs not last
public void test(String... names, int... values) { } // Compilation Error
```

### How does varargs work internally?

Varargs is internally treated as an **array**. For example:

```java
public void show(String... names) { }
```

is interpreted by the compiler as:

```java
public void show(String[] names) { }
```

### What happens when we pass `null` to a varargs method?

If we pass `null`, the method receives a `null` reference, not an empty array — leading to `NullPointerException` if accessed.

```java
show(null); // show(String... names) -> names is null
```

### What is serialization? What is the role of `serialVersionUID`?

* Serialization is the process of converting an object into a byte stream.
* `serialVersionUID` ensures version compatibility during deserialization.

### What is the difference between `String`, `StringBuilder`, and `StringBuffer`?

* `String`: Immutable
* `StringBuilder`: Mutable, not thread-safe
* `StringBuffer`: Mutable, thread-safe (synchronized methods)

## Dependency Injection & Inversion of Control (IoC)

### What is Inversion of Control (IoC)?

**IoC** is a design principle where the control of object creation and dependency management is **inverted from the application to a container/framework**.

In Java (especially Spring), IoC means the **Spring container is responsible** for:

* Instantiating beans
* Injecting dependencies
* Managing bean lifecycle

### What is Dependency Injection (DI)? How does it relate to IoC?

**Dependency Injection** is a specific implementation of IoC where **dependencies are provided to a class**, rather than the class creating them itself.

In Spring, DI is the primary method used to achieve IoC.

### What are the types of Dependency Injection supported in Spring?

1. **Constructor Injection**
2. **Setter Injection**
3. **Field Injection** (via `@Autowired`, not recommended for testing)

### What is the difference between Constructor and Setter Injection?

| Feature             | Constructor Injection | Setter Injection          |
| ------------------- | --------------------- | ------------------------- |
| When used?          | Required dependencies | Optional dependencies     |
| Immutability        | Promotes immutability | Allows mutability         |
| Testing             | Easier to test        | Requires setter methods   |
| Circular Dependency | Fails at runtime      | Can be resolved by Spring |

### How do we enable Dependency Injection in Spring?

Use annotations:

* `@Component`, `@Service`, `@Repository` to define beans
* `@Autowired`, `@Inject`, or `@Qualifier` to inject dependencies
* `@Configuration` + `@Bean` for explicit bean registration

### What are Spring beans?

A **Spring bean** is an object that is **managed by the Spring IoC container**. It is created, configured, and managed by the Spring context.

### What is the role of the ApplicationContext in Spring?

`ApplicationContext` is the **central interface to the Spring IoC container**. It:

* Loads bean definitions
* Wires dependencies
* Manages lifecycle

### What is annotation-based configuration for DI in Spring?

Spring supports annotations to define and inject beans:

* `@Component` - Marks a class as a Spring-managed component
* `@Autowired` - Injects dependencies
* `@Qualifier` - Resolves ambiguity if multiple beans of same type

Example:

```java
@Component
public class Engine {}

@Component
public class Car {
    @Autowired
    private Engine engine;
}
```

### What happens if multiple beans of the same type are available?

Spring will throw `NoUniqueBeanDefinitionException` unless:

* One bean is marked as `@Primary`, **or**
* A specific bean is selected using `@Qualifier("beanName")`

### What is the difference between `@Component`, `@Service`, and `@Repository`?

| Annotation    | Purpose                 | Special Behavior                                                   |
| ------------- | ----------------------- | ------------------------------------------------------------------ |
| `@Component`  | Generic component       | None                                                               |
| `@Service`    | Business logic layer    | Marks service for AOP                                              |
| `@Repository` | Data access layer (DAO) | Enables exception translation (`@PersistenceExceptionTranslation`) |

### Can we inject a collection (List, Set, Map) in Spring?

Yes. Spring can inject a collection of beans using `@Autowired`:

```java
@Autowired
private List<PaymentProcessor> processors;
```

Spring will inject all beans implementing `PaymentProcessor`.

### What is the difference between `@Autowired` and `@Inject`?

| Annotation   | Origin            | Required by default?                 |
| ------------ | ----------------- | ------------------------------------ |
| `@Autowired` | Spring-specific   | Yes                                  |
| `@Inject`    | JSR-330 (Java EE) | Yes (unless used with `@Nullable`)   |

### How does Spring resolve dependencies internally?

Spring maintains a **bean registry**. When a dependency is required, it:

1. Looks up the required type
2. Filters by name (if `@Qualifier` is used)
3. Injects the matching bean instance

### What is circular dependency? How does Spring handle it?

* Occurs when two or more beans depend on each other.
* Spring resolves circular dependencies using **setter injection** (not constructor).
* Constructor injection circular dependencies result in a runtime error.

### Can we inject a bean into a static field?

No. Spring cannot inject into static fields because they are not tied to an instance. We need to inject via constructor, setter, or use `@PostConstruct`.

## Let's Connect

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Follow-blue?logo=linkedin)](https://www.linkedin.com/in/nakul-mitra-microservices-spring-boot-java-postgresql/)
[![Gmail](https://img.shields.io/badge/Gmail-Email-D14836?logo=gmail&logoColor=white)](mailto:nakulmitra2114@gmail.com)