# Singleton Design Pattern in Java

## Table of Contents

1. [What is Singleton?](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#what-is-singleton)
2. [Core Structure](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#core-structure)
3. [6 Ways to Implement Singleton](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#6-ways-to-implement-singleton)
    - [1. Eager Initialization](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#1-eager-initialization)
    - [2. Lazy Initialization (not thread-safe)](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#2-lazy-initialization-not-thread-safe)
    - [3. Synchronized Method](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#3-synchronized-method-thread-safe-but-slow)
    - [4. Double-Checked Locking](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#4-double-checked-locking--recommended)
    - [5. Bill Pugh / Initialization-on-demand Holder](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#5-bill-pugh--initialization-on-demand-holder)
    - [6. Enum Singleton](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#6-enum-singleton)
4. [Bill Pugh — Deep Dive](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#bill-pugh--deep-dive)
5. [Enum Singleton — Deep Dive](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#enum-singleton--deep-dive)
6. [Serialization: The Hidden Singleton Killer](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#serialization-the-hidden-singleton-killer)
7. [Common Pitfalls](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#common-pitfalls)
8. [Quick Comparison](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#quick-comparison)

---

## What is Singleton?

Singleton is a **creational design pattern** that ensures a class has **only one instance** throughout the application's lifetime, and provides a global access point to that instance.

**When to use it:**

- Database connections
- Logging
- Configuration managers
- Thread pools
- Caches

Anywhere you want exactly one shared resource.

---

## Core Structure

Every singleton implementation needs three things:

1. A `private static` field holding the single instance
2. A `private` constructor (blocks `new ClassName()` from outside)
3. A `public static` method to get that instance

```
┌──────────────────────────────────────┐
│            Singleton                 │
├──────────────────────────────────────┤
│ - instance: Singleton  (static)      │
│ - Singleton()          (private)     │
├──────────────────────────────────────┤
│ + getInstance(): Singleton (static)  │
│ + businessLogic(): void              │
└──────────────────────────────────────┘
         ▲
         │ getInstance()
    ┌────┴────┐
    │ Client  │
    └─────────┘
```

---

## 6 Ways to Implement Singleton

### 1. Eager Initialization

The instance is created when the class loads — simplest approach, but wastes memory if the object is never used.

```java
public class Singleton {
    // Created at class load time
    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

**Pros:** Thread-safe (JVM guarantees class loading is atomic), simple.  
**Cons:** Instance is created even if never used.

---

### 2. Lazy Initialization (not thread-safe)

The instance is created only when first needed.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();  // ⚠️ Race condition possible here
        }
        return instance;
    }
}
```

**Pros:** Memory-efficient.  
**Cons:** Broken in multi-threaded environments — two threads can both see `null` simultaneously and create two instances.

> **Never use this in production.**

---

### 3. Synchronized Method (thread-safe, but slow)

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**Pros:** Thread-safe.  
**Cons:** `synchronized` is acquired on _every_ call — even after the instance exists. This is a performance bottleneck under high load.

---

### 4. Double-Checked Locking ✅ (recommended)

Synchronizes only the first creation, not every subsequent call.

```java
public class Singleton {
    // volatile prevents instruction reordering issues
    // Note : study this instruction reordering issue
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {                    // First check (no lock)
            synchronized (Singleton.class) {
                if (instance == null) {            // Second check (with lock)
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

The `volatile` keyword is **critical** here — without it, the JVM can reorder instructions and another thread might see a partially constructed object.

**Pros:** Thread-safe + efficient after first creation.  
**Cons:** Slightly verbose. Requires Java 5+ for `volatile` to work correctly.

---

### 5. Bill Pugh / Initialization-on-demand Holder

Leverages the JVM's class loading guarantee — the inner class is only loaded when `getInstance()` is called.

```java
public class Singleton {

    private Singleton() {}

    // Inner class is loaded lazily and only once by the JVM
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

**Pros:** Thread-safe without `synchronized` or `volatile`, lazy initialization, clean code.  
**Cons:** None significant. This is the most widely recommended approach.

---

### 6. Enum Singleton

The most robust approach — Java's `enum` guarantees a single instance by specification, and it's automatically serialization-safe.

```java
public enum Singleton {
    INSTANCE;

    public void doSomething() {
        System.out.println("Singleton via Enum");
    }
}

// Usage:
Singleton.INSTANCE.doSomething();
```

**Pros:** Thread-safe, serialization-safe, reflection-safe, concise.  
**Cons:** Can't extend another class (enums can't inherit). Less intuitive if team isn't familiar with the pattern.

---

## Bill Pugh — Deep Dive

### The core idea

The trick is hiding the instance inside a **static nested class**. Java's class loader only loads a class when it's first _actively used_ — not when the outer class loads.

```java
public class Singleton {

    private Singleton() {
        System.out.println("Instance created!");
    }

    private static class SingletonHolder {
        // This line runs ONLY when SingletonHolder is first accessed
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;  // triggers holder class load
    }
}
```

### How the JVM guarantees this

The JVM spec (§12.4) says a class is initialized **exactly once**, and initialization is **synchronized by the JVM itself** at the class loader level. You get thread-safety for free — no `synchronized`, no `volatile` needed.

**Sequence of events:**

```
1. App starts
        │
2. Singleton.class loads
   └─ Private constructor registered. No instance created.
        │
3. getInstance() called (first time)
   └─ First access to SingletonHolder.INSTANCE
        │
4. JVM acquires class initialization lock  ← THE KEY STEP
   └─ All other threads block until initialization completes
        │
5. SingletonHolder.class initializes
   └─ INSTANCE = new Singleton() runs exactly once
        │
6. JVM releases lock → returns INSTANCE
   └─ All subsequent calls: direct field read, zero synchronization
```

The amber step above is what makes this thread-safe — the JVM's own class initialization lock handles it internally. After that first initialization, `getInstance()` is just a direct field read with zero synchronization overhead on every subsequent call.

### Proving it's lazy — a demo

```java
public class Singleton {

    private Singleton() {
        System.out.println("Singleton constructor called!");
    }

    private static class SingletonHolder {
        static {
            // This runs when SingletonHolder loads
            System.out.println("SingletonHolder class loaded!");
        }
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }

    public static void someOtherMethod() {
        System.out.println("someOtherMethod called — no instance created yet");
    }
}

// Main:
public class Main {
    public static void main(String[] args) {
        System.out.println("App started");
        Singleton.someOtherMethod();           // Does NOT trigger holder
        System.out.println("Now calling getInstance...");
        Singleton s = Singleton.getInstance(); // Triggers holder HERE
    }
}
```

**Output:**

```
App started
someOtherMethod called — no instance created yet
Now calling getInstance...
SingletonHolder class loaded!
Singleton constructor called!
```

The constructor only fires when `getInstance()` is actually called — proving true lazy initialization.

---

## Enum Singleton — Deep Dive

### How it works

`enum` types in Java are full classes under the hood. The JVM treats each enum constant as a `public static final` field that's initialized exactly once during class loading — the same guarantee as eager initialization, but with much stronger protections.

```java
public enum DatabaseConnection {
    INSTANCE;

    private final String url;
    private Connection connection;

    // Enum constructor is implicitly private
    DatabaseConnection() {
        this.url = "jdbc:postgresql://localhost/mydb";
        System.out.println("DB connection singleton created");
    }

    public Connection getConnection() {
        if (connection == null) {
            // initialize connection
        }
        return connection;
    }

    public void query(String sql) {
        System.out.println("Running: " + sql);
    }
}

// Usage:
DatabaseConnection.INSTANCE.query("SELECT * FROM users");
DatabaseConnection db = DatabaseConnection.INSTANCE;
```

### What the JVM/compiler actually generates

When you write an `enum`, the compiler secretly generates something equivalent to this:

```java
// What the compiler generates behind the scenes (simplified)
public final class DatabaseConnection {

    public static final DatabaseConnection INSTANCE;

    static {
        INSTANCE = new DatabaseConnection();  // Once, at class load
    }

    // Implicitly private
    private DatabaseConnection() {
        this.url = "jdbc:postgresql://localhost/mydb";
    }

    // PLUS: Java bakes in these protections automatically ↓
    // - Blocks reflection
    // - Blocks cloning
    // - Blocks deserialization creating new instances
}
```

### The three built-in protections enum gives you

**1. Reflection-proof**

`java.lang.reflect.Constructor` throws `IllegalArgumentException` if you try to instantiate an enum via reflection. It's hardcoded into the JDK.

```java
// This FAILS with IllegalArgumentException:
Constructor<DatabaseConnection> c =
    DatabaseConnection.class.getDeclaredConstructor();
c.setAccessible(true);
c.newInstance();  // throws!
```

**2. Clone-proof**

`Enum` base class declares `clone()` as `final` and throws `CloneNotSupportedException`. You can't override it.

**3. Serialization-proof**

Java's serialization mechanism treats enums specially. It serializes only the name (`"INSTANCE"`) and uses `Enum.valueOf()` to look up the existing constant during deserialization — it never calls the constructor. This is the biggest advantage over other approaches.

---

## Serialization: The Hidden Singleton Killer

### The problem — what actually happens during deserialization

Normally Java serialization works by writing all the object's fields to a byte stream, then reconstructing the object on the other end by calling `ObjectInputStream` — which **bypasses the constructor entirely** and directly allocates memory and sets fields via native code.

This means your singleton becomes **not a singleton** the moment you serialize and deserialize it:

```java
public class Singleton implements Serializable {
    private static final long serialVersionUID = 1L;

    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}

// Breaking it:
public class Main {
    public static void main(String[] args) throws Exception {
        Singleton s1 = Singleton.getInstance();

        // Serialize to file
        ObjectOutputStream out = new ObjectOutputStream(
            new FileOutputStream("singleton.ser"));
        out.writeObject(s1);
        out.close();

        // Deserialize from file
        ObjectInputStream in = new ObjectInputStream(
            new FileInputStream("singleton.ser"));
        Singleton s2 = (Singleton) in.readObject();
        in.close();

        System.out.println(s1 == s2);       // false  ← singleton is BROKEN
        System.out.println(s1.hashCode());  // e.g. 12345678
        System.out.println(s2.hashCode());  // e.g. 87654321 ← different object!
    }
}
```

Two different instances exist in memory. The singleton contract is violated.

### What's happening under the hood

```
WITHOUT readResolve                    WITH readResolve
──────────────────────                 ─────────────────────────
Singleton.getInstance()                Singleton.getInstance()
        │                                      │
Serialize → file                       Serialize → file
(writes field bytes to stream)         (writes field bytes to stream)
        │                                      │
ObjectInputStream.readObject()         ObjectInputStream.readObject()
(allocates new memory,                 (allocates new memory,
 skips constructor)                     skips constructor)
        │                                      │
NEW instance returned                  readResolve() called by JVM
        │                              (returns existing INSTANCE,
s1 == s2 → FALSE ✗                      discards new object)
Two objects in heap                            │
                                       s1 == s2 → TRUE ✓
                                       One object in heap
```

### The fix — `readResolve()`

`readResolve()` is a special hook that Java's `ObjectInputStream` looks for via reflection. If the class being deserialized declares this method, the JVM calls it **after** constructing the new object, and uses the **returned value** instead — then the newly created object gets garbage collected.

```java
public class Singleton implements Serializable {
    private static final long serialVersionUID = 1L;

    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }

    // This method is called by ObjectInputStream after deserialization
    protected Object readResolve() {
        // Throw away the newly created object, return the real singleton
        return INSTANCE;
    }
}
```

### Full working demo with proof

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws Exception {
        Singleton s1 = Singleton.getInstance();
        System.out.println("Before serialization: " + s1.hashCode());

        // Serialize
        try (ObjectOutputStream out = new ObjectOutputStream(
                new FileOutputStream("singleton.ser"))) {
            out.writeObject(s1);
        }

        // Deserialize
        Singleton s2;
        try (ObjectInputStream in = new ObjectInputStream(
                new FileInputStream("singleton.ser"))) {
            s2 = (Singleton) in.readObject();
        }

        System.out.println("After deserialization: " + s2.hashCode());
        System.out.println("Same instance? " + (s1 == s2));
    }
}

// Without readResolve:
// Before serialization:   12345678
// After deserialization:  87654321
// Same instance? false              ← BROKEN

// With readResolve:
// Before serialization:   12345678
// After deserialization:  12345678
// Same instance? true               ← CORRECT
```

### Why `protected` and not `private`?

```java
// If your singleton has subclasses (unusual but possible):
protected Object readResolve() { return INSTANCE; }
// protected → subclasses inherit this fix too

// If it's a final class (most singletons are):
private Object readResolve() { return INSTANCE; }
// private is fine — no subclasses possible
```

---

## Common Pitfalls

### Reflection attack

Someone can call the private constructor via `Constructor.setAccessible(true)`:

```java
Constructor<Singleton> c = Singleton.class.getDeclaredConstructor();
c.setAccessible(true);
Singleton s2 = c.newInstance();  // Bypasses private constructor!
```

**Fix:** Throw an exception inside the constructor if the instance already exists:

```java
private Singleton() {
    if (INSTANCE != null) {
        throw new IllegalStateException("Use getInstance()");
    }
}
```

Or simply use the **Enum approach**, which blocks this at the JDK level.

### Serialization (without readResolve)

As covered above — Java creates a new instance during deserialization. Fix by adding `readResolve()` or using **Enum**.

### Cloning

If your class extends a class that implements `Cloneable`, `clone()` can bypass the singleton:

```java
// Fix: override clone() and throw
@Override
protected Object clone() throws CloneNotSupportedException {
    throw new CloneNotSupportedException("Singleton cannot be cloned");
}
```

---

## Quick Comparison

|Approach|Thread-safe|Lazy|Serialization-safe|Reflection-safe|Recommended|
|---|---|---|---|---|---|
|Eager|✅|❌|❌ (needs readResolve)|❌|For lightweight objects|
|Lazy (naive)|❌|✅|❌|❌|❌ Never in production|
|Synchronized method|✅|✅|❌|❌|❌ Avoid (slow)|
|Double-checked locking|✅|✅|❌|❌|✅ Good choice|
|Bill Pugh holder|✅|✅|❌ (needs readResolve)|❌|✅ Most elegant|
|Enum|✅|❌|✅ built-in|✅ built-in|✅ Most robust|

### When to use which

|Situation|Recommendation|
|---|---|
|Simple, no serialization needed|**Bill Pugh holder** — cleanest code|
|Needs to be serializable|**Enum** — serialization handled automatically|
|Needs to extend another class|**Bill Pugh holder** + add `readResolve()`|
|Maximum robustness, no inheritance needed|**Enum** — reflection + clone + serialization all blocked by JVM|

---

> The reason Joshua Bloch (author of _Effective Java_) recommends the Enum approach is precisely because it makes all three attack vectors — reflection, cloning, and serialization — a non-problem. You get correctness for free from the language spec itself, rather than relying on you remembering to add `readResolve()`.