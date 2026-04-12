# Factory Design Pattern (Java + Haskell)

## 🧠 What is Factory Pattern?

The **Factory Pattern** is a creational design pattern that provides a way to create objects **without exposing the instantiation logic** to the client.

Instead of directly using constructors (`new`), object creation is delegated to a **factory method**.

The **Factory Method** provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created. It's like having a generic "Product Creator" where the specific item being manufactured is decided by the specific factory floor.

---

## 🎯 Problem Statement

When you write code like:

```java
Shape shape = new Circle();
```

You tightly couple your code to a specific implementation (`Circle`).

### Issues:
- Hard to extend
- Violates Open/Closed Principle
- Difficult to test
- Scattered object creation logic

---

## 💡 Solution

Encapsulate object creation inside a **Factory**.

```java
Shape shape = ShapeFactory.getShape("circle");
```

Now:
- Client doesn't know the concrete class
- Creation logic is centralized

---

# ☕ Java Implementation

## 1. Define Interface

```java
interface Shape {
    void draw();
}
```

## 2. Concrete Classes

```java
class Circle implements Shape {
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

class Square implements Shape {
    public void draw() {
        System.out.println("Drawing Square");
    }
}
```

## 3. Factory Class

```java
class ShapeFactory {

    public static Shape getShape(String type) {
        switch (type.toLowerCase()) {
            case "circle":
                return new Circle();
            case "square":
                return new Square();
            default:
                throw new IllegalArgumentException("Unknown shape type");
        }
    }
}
```

## 4. Usage

```java
public class Main {
    public static void main(String[] args) {
        Shape shape = ShapeFactory.getShape("circle");
        shape.draw();
    }
}
```

---

## ⚡ Key Advantages (Java)

- Loose coupling
- Centralized object creation
- Easier to extend
- Improves testability

---

## ⚠️ Drawbacks

- Switch/if-else can grow large
- Violates Open/Closed Principle (needs modification for new types)

---

## 🚀 Improved Factory (Map-based)

```java
import java.util.HashMap;
import java.util.Map;
import java.util.function.Supplier;

class ShapeFactory {

    private static final Map<String, Supplier<Shape>> registry = new HashMap<>();

    static {
        registry.put("circle", Circle::new);
        registry.put("square", Square::new);
    }

    public static Shape getShape(String type) {
        Supplier<Shape> supplier = registry.get(type.toLowerCase());
        if (supplier != null) return supplier.get();
        throw new IllegalArgumentException("Unknown shape type");
    }
}
```

---

# 🟣 Haskell Implementation

## 🧠 Functional Perspective

Haskell does not use classes or inheritance.

Factory pattern is implemented using:
- Algebraic Data Types
- Pure functions
- Pattern matching

---

## 1. Define Data Type

```haskell
data Shape = Circle | Square
```

## 2. Behavior Function

```haskell
draw :: Shape -> String
draw Circle = "Drawing Circle"
draw Square = "Drawing Square"
```

## 3. Factory Function

```haskell
createShape :: String -> Maybe Shape
createShape "circle" = Just Circle
createShape "square" = Just Square
createShape _ = Nothing
```

## 4. Usage

```haskell
main :: IO ()
main = do
    let shape = createShape "circle"
    case shape of
        Just s  -> putStrLn (draw s)
        Nothing -> putStrLn "Invalid shape"
```

---

## ⚡ Key Advantages (Haskell)

- Type-safe (no nulls)
- No runtime polymorphism issues
- Compile-time guarantees
- Simple and declarative

---

# 🔥 Java vs Haskell Comparison

| Concept | Java | Haskell |
|--------|------|--------|
| Paradigm | OOP | Functional |
| Object Creation | Factory class | Function |
| Polymorphism | Runtime | Compile-time |
| Safety | Nullable | Maybe type |

---

# 🌍 Real-World Use Cases

- Payment systems (UPI, Card, NetBanking)
- Logging frameworks
- Notification systems (Email, SMS, Push)
- Parser selection

---

# 🧠 Interview Notes

### When to use Factory?
- When object creation logic is complex
- When you want loose coupling
- When you have multiple implementations

### Common Follow-up Questions

**Q: Does Factory violate OCP?**
- Basic version: Yes
- Improved version: No (using registration/map)

**Q: Difference between Factory and Abstract Factory?**
- Factory → creates one object
- Abstract Factory → creates families of objects

---

# 🧪 Practice Exercise

Implement:

### Java
- Interface: Vehicle
- Classes: Car, Bike
- Factory: VehicleFactory

### Haskell
- Data type: Vehicle
- Function: createVehicle

---

# 📌 Summary

- Factory Pattern abstracts object creation
- Promotes loose coupling
- Centralizes logic
- Functional equivalent = pure functions + ADTs

---

If you want, next we can add:
- Abstract Factory
- Builder
- Singleton
- Prototype

in the same structured format.

