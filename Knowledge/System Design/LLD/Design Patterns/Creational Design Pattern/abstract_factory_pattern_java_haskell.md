# Abstract Factory Pattern (Java + Haskell)

## 🧠 What is Abstract Factory Pattern?

The **Abstract Factory Pattern** is a creational design pattern that provides an interface for creating **families of related or dependent objects** without specifying their concrete classes.

👉 It is often described as a **"Factory of Factories"**.

---

## 🎯 Problem Statement

Consider a UI system with themes:

- Light Theme → LightButton, LightTextBox
- Dark Theme → DarkButton, DarkTextBox

### Problem:
- You must ensure consistent combinations
- Avoid mixing LightButton with DarkTextBox

Using a simple Factory can lead to **inconsistent object creation**.

---

## 💡 Solution

Create a factory for each family:

```java
UIFactory factory = new DarkThemeFactory();
Button btn = factory.createButton();
TextBox txt = factory.createTextBox();
```

Now, the system guarantees consistency.

---

# ☕ Java Implementation

## 1. Abstract Products

```java
interface Button {
    void render();
}

interface TextBox {
    void render();
}
```

---

## 2. Concrete Products

### Light Theme

```java
class LightButton implements Button {
    public void render() {
        System.out.println("Light Button");
    }
}

class LightTextBox implements TextBox {
    public void render() {
        System.out.println("Light TextBox");
    }
}
```

### Dark Theme

```java
class DarkButton implements Button {
    public void render() {
        System.out.println("Dark Button");
    }
}

class DarkTextBox implements TextBox {
    public void render() {
        System.out.println("Dark TextBox");
    }
}
```

---

## 3. Abstract Factory

```java
interface UIFactory {
    Button createButton();
    TextBox createTextBox();
}
```

---

## 4. Concrete Factories

```java
class LightThemeFactory implements UIFactory {
    public Button createButton() {
        return new LightButton();
    }

    public TextBox createTextBox() {
        return new LightTextBox();
    }
}

class DarkThemeFactory implements UIFactory {
    public Button createButton() {
        return new DarkButton();
    }

    public TextBox createTextBox() {
        return new DarkTextBox();
    }
}
```

---

## 5. Client Code

```java
public class Main {
    public static void main(String[] args) {

        UIFactory factory = new DarkThemeFactory();

        Button button = factory.createButton();
        TextBox textBox = factory.createTextBox();

        button.render();
        textBox.render();
    }
}
```

---

## ⚡ Key Advantages (Java)

- Ensures consistency of object families
- Promotes loose coupling
- Easy to switch entire configurations

---

## ⚠️ Drawbacks

- Hard to extend when adding new product types
- Requires modifying all factories

---

# 🟣 Haskell Implementation

## 🧠 Functional Perspective

Haskell replaces classes and inheritance with:
- Algebraic Data Types
- Records
- Functions

---

## 1. Product Types

```haskell
data Button = Button { renderButton :: IO () }
data TextBox = TextBox { renderTextBox :: IO () }
```

---

## 2. Concrete Implementations

```haskell
lightButton :: Button
lightButton = Button {
    renderButton = putStrLn "Light Button"
}

lightTextBox :: TextBox
lightTextBox = TextBox {
    renderTextBox = putStrLn "Light TextBox"
}

darkButton :: Button
darkButton = Button {
    renderButton = putStrLn "Dark Button"
}

darkTextBox :: TextBox
darkTextBox = TextBox {
    renderTextBox = putStrLn "Dark TextBox"
}
```

---

## 3. Abstract Factory

```haskell
data UIFactory = UIFactory {
    createButton :: Button,
    createTextBox :: TextBox
}
```

---

## 4. Concrete Factories

```haskell
lightFactory :: UIFactory
lightFactory = UIFactory {
    createButton = lightButton,
    createTextBox = lightTextBox
}

darkFactory :: UIFactory
darkFactory = UIFactory {
    createButton = darkButton,
    createTextBox = darkTextBox
}
```

---

## 5. Usage

```haskell
main :: IO ()
main = do
    let factory = darkFactory
    renderButton (createButton factory)
    renderTextBox (createTextBox factory)
```

---

## ⚡ Key Advantages (Haskell)

- Type-safe
- No nulls (can extend with Maybe)
- Declarative and simple

---

# 🔥 Factory vs Abstract Factory

| Feature | Factory | Abstract Factory |
|--------|--------|----------------|
| Scope | Single object | Family of objects |
| Complexity | Low | Higher |
| Consistency | Not guaranteed | Guaranteed |

---

# 🌍 Real-World Use Cases

- Cross-platform UI systems
- Payment gateways (country-specific)
- Database drivers

---

# 🧠 Interview Notes

## When to use?
- When objects must be used together
- When you need consistency across products

## Common Questions

**Q: What happens when adding a new product (e.g., Checkbox)?**
- Must update all factories ❌

**Q: What happens when adding a new family (e.g., Blue theme)?**
- Just add a new factory ✅

---

# 🧪 Practice Exercise

## Java
- VehicleFactory
    - CarFactory → Car + Engine
    - BikeFactory → Bike + Engine

## Haskell
- Same concept using records

---

# 📌 Summary

- Abstract Factory creates families of related objects
- Prevents incompatible combinations
- Useful in scalable and configurable systems

---

Next: Builder Pattern 🚀
