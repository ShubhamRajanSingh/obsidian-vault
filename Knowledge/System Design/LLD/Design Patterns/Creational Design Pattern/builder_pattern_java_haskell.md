# Builder Pattern (Java + Haskell)

## 🧠 What is Builder Pattern?

The **Builder Pattern** is a creational design pattern used to construct **complex objects step-by-step**.

It is especially useful when an object has:
- Many optional parameters
- Complex construction logic

---

## 🎯 Problem Statement

Using constructors with many parameters leads to unreadable and error-prone code:

```java
User user = new User("Shubham", 26, "India", null, null, true);
```

### Issues:
- Hard to read
- Parameter order matters
- Difficult to maintain
- Confusing optional fields

---

## 💡 Solution

Use a **Builder** to construct objects step-by-step:

```java
User user = new User.Builder()
    .setName("Shubham")
    .setAge(26)
    .setCountry("India")
    .build();
```

---

# ☕ Java Implementation

## 1. Class with Private Constructor

```java
class User {
    private final String name;
    private final int age;
    private final String country;

    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.country = builder.country;
    }
```

---

## 2. Static Builder Class

```java
    public static class Builder {
        private String name;
        private int age;
        private String country;

        public Builder setName(String name) {
            this.name = name;
            return this;
        }

        public Builder setAge(int age) {
            this.age = age;
            return this;
        }

        public Builder setCountry(String country) {
            this.country = country;
            return this;
        }

        public User build() {
            if (name == null) {
                throw new IllegalStateException("Name is required");
            }
            return new User(this);
        }
    }
}
```

---

## 3. Usage

```java
User user = new User.Builder()
    .setName("Shubham")
    .setAge(26)
    .setCountry("India")
    .build();
```

---

## ⚡ Key Advantages (Java)

- Improves readability
- Supports optional parameters
- Encourages immutability (using `final` fields)
- Enables validation before object creation
- Supports method chaining (fluent API)

---

## ⚠️ Drawbacks

- More boilerplate code
- Slightly more complex than constructors

---

## 🚀 Advanced Tips

### 1. Required Fields in Builder Constructor

```java
public Builder(String name) {
    this.name = name;
}
```

### 2. Immutability Best Practice

```java
private final String name;
```

### 3. Thread Safety
- Built object is thread-safe if immutable
- Builder itself is not thread-safe (acceptable)

---

# 🟣 Haskell Implementation

## 🧠 Functional Perspective

Haskell avoids builders by using:
- Immutable data
- Default values
- Function composition

---

## 1. Define Data Type

```haskell
data User = User {
    name :: String,
    age :: Int,
    country :: String
} deriving Show
```

---

## 2. Default Object

```haskell
defaultUser :: User
defaultUser = User {
    name = "",
    age = 0,
    country = ""
}
```

---

## 3. Update Functions

```haskell
setName :: String -> User -> User
setName n user = user { name = n }

setAge :: Int -> User -> User
setAge a user = user { age = a }

setCountry :: String -> User -> User
setCountry c user = user { country = c }
```

---

## 4. Usage (Function Composition)

```haskell
main :: IO ()
main = do
    let user = setCountry "India"
             . setAge 26
             . setName "Shubham"
             $ defaultUser

    print user
```

---

## ⚡ Key Advantages (Haskell)

- Immutable by default
- No need for builder class
- Simple and expressive
- Compile-time safety

---

# 🔥 Java vs Haskell Comparison

| Concept | Java | Haskell |
|--------|------|--------|
| Mutability | Builder mutable, object immutable | Fully immutable |
| Pattern Need | High | Low |
| Style | Fluent API | Function composition |
| Safety | Runtime checks | Compile-time safety |

---

# 🌍 Real-World Use Cases

- HTTP request builders
- Database query builders
- Configuration objects
- Lombok `@Builder`

---

# 🧠 Interview Notes

## When to use?
- Many optional parameters
- Need readable object creation
- Require immutability

## Common Questions

**Q: Why not constructors?**
- Hard to read and maintain

**Q: Builder vs Factory?**
- Builder → step-by-step construction
- Factory → single-step creation

**Q: Should fields be final?**
- Not mandatory, but strongly recommended for immutability

---

# 🧪 Practice Exercise

## Java
- Implement `Pizza` builder:
  - size (required)
  - cheese
  - toppings
  - crust type

## Haskell
- Same using record updates

---

# 📌 Summary

- Builder Pattern simplifies complex object creation
- Promotes readability and immutability
- Widely used in real-world APIs

---

Next: Singleton Pattern 🚀

