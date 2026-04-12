# Prototype Design Pattern in Java

## Table of Contents

1. [What is Prototype Pattern?](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#what-is-prototype-pattern)
2. [Core Structure — the four roles](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#core-structure--the-four-roles)
3. [Shallow Copy vs Deep Copy](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#shallow-copy-vs-deep-copy)
4. [5 Implementation Approaches](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#5-implementation-approaches)
    - [1. Cloneable — shallow copy](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#1-cloneable--shallow-copy)
    - [2. Cloneable — deep copy](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#2-cloneable--deep-copy)
    - [3. Copy Constructor](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#3-copy-constructor-preferred)
    - [4. Prototype Registry](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#4-prototype-registry)
    - [5. Serialization-based deep copy](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#5-serialization-based-deep-copy)
5. [Real-world Example — Game Character System](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#real-world-example--game-character-system)
6. [clone() with Nested Objects — Does it Work?](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#clone-with-nested-objects--does-it-work)
7. [Common Pitfalls](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#common-pitfalls)
8. [Prototype vs Factory](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#prototype-vs-factory)
9. [Quick Summary](https://claude.ai/chat/fac1069b-decc-42cc-a8b5-e255ce76ef5f#quick-summary)

---

## What is Prototype Pattern?

Prototype is a **creational design pattern** where you create new objects by **cloning an existing object** (the prototype) rather than constructing a new one from scratch using `new`.

The key idea: instead of calling `new ClassName()` and re-doing all the configuration, you copy an already-configured object and adjust only what differs.

**When to use it:**

- Object creation is expensive (DB calls, network calls, heavy computation)
- You need many similar objects with slight variations
- You want to avoid subclass explosion just to configure objects differently
- The construction logic is complex and you don't want to repeat it across the codebase

---

## Core Structure — the four roles

```
┌──────────┐          uses          ┌────────────────────────────────┐
│  Client  │ ─────────────────────> │  «interface» Prototype         │
└──────────┘                        │  + clone(): Prototype          │
                                    └────────────────────────────────┘
                                          ▲                ▲
                                   implements        implements
                                          │                │
                              ┌──────────────────┐  ┌──────────────────┐
                              │ ConcretePrototype1│  │ ConcretePrototype2│
                              │ + clone()         │  │ + clone()         │
                              │   return copy of  │  │   return copy of  │
                              │   this            │  │   this            │
                              └──────────────────┘  └──────────────────┘
```

|Role|Responsibility|
|---|---|
|`Prototype`|Interface/abstract class declaring `clone()`|
|`ConcretePrototype`|Implements `clone()` — returns a copy of itself|
|`Client`|Calls `clone()` without knowing the concrete type|
|`Registry` _(optional)_|Stores named prototypes; clients request clones by key|

---

## Shallow Copy vs Deep Copy

This is the most critical concept in Prototype pattern and the source of most bugs.

Java's `Object.clone()` gives a **shallow copy** by default — primitive fields are duplicated, but **object references are shared** between original and clone.

```
SHALLOW COPY                        DEEP COPY

┌────────────────┐                  ┌────────────────┐
│ Original       │                  │ Original       │
│  id = 1        │                  │  id = 1        │
│  address ──────┼──┐               │  address ──────┼──> [Address A: "Mumbai"]
└────────────────┘  │               └────────────────┘
                    ▼
              [Address: "Mumbai"]    ┌────────────────┐
                    ▲               │ Clone          │
┌────────────────┐  │               │  id = 1        │
│ Clone          │  │               │  address ──────┼──> [Address B: "Mumbai"]
│  id = 1        │  │               └────────────────┘
│  address ──────┼──┘
└────────────────┘                  Both are fully independent.
                                    Changing clone.address does NOT
Both share the same Address.        affect original.
Changing clone.address ALSO
changes original. Bug!
```

**Rule of thumb:** If your object contains only primitives and `String`s (which are immutable), shallow copy is safe. If it contains any mutable reference types (`List`, `Map`, custom objects), you need a deep copy.

---

## 5 Implementation Approaches

### 1. Cloneable — Shallow Copy

Java's built-in mechanism. The class must implement the `Cloneable` marker interface and override `clone()`.

```java
public class Employee implements Cloneable {
    private int id;
    private String name;
    private Address address;  // mutable nested object — danger zone

    public Employee(int id, String name, Address address) {
        this.id = id;
        this.name = name;
        this.address = address;
    }

    @Override
    public Employee clone() {
        try {
            return (Employee) super.clone();  // shallow copy only
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e);
        }
    }
}

public class Address {
    public String city;
    public Address(String city) { this.city = city; }
}

// The problem:
Address addr = new Address("Mumbai");
Employee original = new Employee(1, "Ravi", addr);
Employee clone = original.clone();

clone.address.city = "Pune";  // mutates BOTH — address reference is shared

System.out.println(original.address.city); // "Pune"  ← BROKEN
System.out.println(clone.address.city);    // "Pune"
```

**Pros:** Simple, built into Java.  
**Cons:** Shallow by default — mutable nested fields are shared. `CloneNotSupportedException` is checked and must be handled. `Cloneable` interface itself is widely considered a design mistake in Java.

> **Do not use this approach if your object has any mutable reference fields.**

---

### 2. Cloneable — Deep Copy

Fix the shallow copy by manually cloning every mutable nested object inside `clone()`.

```java
public class Employee implements Cloneable {
    private int id;
    private String name;
    private Address address;
    private List<String> skills;

    public Employee(int id, String name, Address address, List<String> skills) {
        this.id = id;
        this.name = name;
        this.address = address;
        this.skills = skills;
    }

    @Override
    public Employee clone() {
        try {
            Employee copy = (Employee) super.clone();
            // Manually deep-copy every mutable field
            copy.address = new Address(this.address.city);
            copy.skills = new ArrayList<>(this.skills);
            return copy;
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e);
        }
    }
}

// Now safe:
Employee original = new Employee(1, "Ravi", new Address("Mumbai"),
                                 new ArrayList<>(List.of("Java", "Spring")));
Employee clone = original.clone();

clone.address.city = "Pune";
clone.skills.add("Kafka");

System.out.println(original.address.city); // "Mumbai" ← unchanged
System.out.println(original.skills);       // [Java, Spring] ← unchanged
System.out.println(clone.address.city);    // "Pune"   ← independent
System.out.println(clone.skills);          // [Java, Spring, Kafka]
```

**Pros:** Correct deep copy behavior.  
**Cons:** You must remember to clone every mutable field manually — easy to forget when the class evolves. Still tied to the flawed `Cloneable` interface.

---

### 3. Copy Constructor (preferred)

Avoids `Cloneable` entirely by writing a constructor that accepts an instance of the same class. This is the most explicit, readable, and widely recommended approach.

```java
public class Employee {
    private int id;
    private String name;
    private Address address;
    private List<String> skills;

    // Normal constructor
    public Employee(int id, String name, Address address, List<String> skills) {
        this.id = id;
        this.name = name;
        this.address = address;
        this.skills = skills;
    }

    // Copy constructor — explicit, no checked exceptions, no magic
    public Employee(Employee other) {
        this.id = other.id;
        this.name = other.name;
        this.address = new Address(other.address.city);  // deep copy
        this.skills = new ArrayList<>(other.skills);     // deep copy
    }

    // Optional convenience method
    public Employee copy() {
        return new Employee(this);
    }
}

public class Address {
    public String city;
    public Address(String city) { this.city = city; }

    // Address also has a copy constructor
    public Address(Address other) { this.city = other.city; }
}

// Usage:
Employee original = new Employee(1, "Ravi",
    new Address("Mumbai"), new ArrayList<>(List.of("Java")));

Employee clone1 = new Employee(original);  // explicit copy constructor call
Employee clone2 = original.copy();         // convenience method

clone1.address.city = "Pune";
System.out.println(original.address.city); // "Mumbai" — safe
```

**Pros:** No `Cloneable` baggage, no checked exceptions, completely explicit about what gets copied. Easy to review. Works well with immutable classes and records.  
**Cons:** You must define the copy constructor yourself on every class in the object graph.

---

### 4. Prototype Registry

In real applications you usually maintain a **registry** — a map of pre-configured, named prototype instances. Clients request a clone by key, without knowing the concrete class at all.

```java
// Step 1: Define the Prototype interface
public interface Prototype {
    Prototype clone();
}

// Step 2: A concrete prototype with copy constructor
public class GameCharacter implements Prototype {
    private String role;
    private int health;
    private int attackPower;
    private List<String> skills;

    public GameCharacter(String role, int health, int attackPower, List<String> skills) {
        this.role = role;
        this.health = health;
        this.attackPower = attackPower;
        this.skills = skills;
    }

    // Private copy constructor used by clone()
    private GameCharacter(GameCharacter other) {
        this.role = other.role;
        this.health = other.health;
        this.attackPower = other.attackPower;
        this.skills = new ArrayList<>(other.skills);  // deep copy
    }

    @Override
    public GameCharacter clone() {
        return new GameCharacter(this);
    }

    public void setHealth(int h) { this.health = h; }
    public void addSkill(String s) { this.skills.add(s); }

    @Override
    public String toString() {
        return role + " | HP:" + health + " | ATK:" + attackPower + " | " + skills;
    }
}

// Step 3: The Registry
public class PrototypeRegistry {
    private final Map<String, Prototype> registry = new HashMap<>();

    public void register(String key, Prototype prototype) {
        registry.put(key, prototype);
    }

    // Always returns a CLONE — never the stored prototype itself
    public Prototype get(String key) {
        Prototype proto = registry.get(key);
        if (proto == null) {
            throw new IllegalArgumentException("Unknown prototype: " + key);
        }
        return proto.clone();
    }
}

// Step 4: Wire it up
public class Main {
    public static void main(String[] args) {
        PrototypeRegistry registry = new PrototypeRegistry();

        // Register expensive-to-create base configurations once
        registry.register("warrior",
            new GameCharacter("Warrior", 200, 80, List.of("Slash", "Shield Bash")));
        registry.register("mage",
            new GameCharacter("Mage", 100, 150, List.of("Fireball", "Teleport")));

        // Clients clone and customise — no 'new GameCharacter(...)' needed
        GameCharacter player1 = (GameCharacter) registry.get("warrior");
        player1.setHealth(180);  // player took some damage
        player1.addSkill("Parry");

        GameCharacter player2 = (GameCharacter) registry.get("warrior");
        // player2 is fresh — unaffected by player1's mutations

        System.out.println(player1);
        // Warrior | HP:180 | ATK:80 | [Slash, Shield Bash, Parry]

        System.out.println(player2);
        // Warrior | HP:200 | ATK:80 | [Slash, Shield Bash]
    }
}
```

This is the most complete and realistic use of the Prototype pattern. The registry decouples clients from concrete types entirely — you can add new character types without changing any client code.

---

### 5. Serialization-based Deep Copy

When you have a deeply nested object graph and writing manual clone logic for every class is impractical, Java serialization can do a full deep copy automatically.

```java
import java.io.*;

public class DeepCopyUtil {

    @SuppressWarnings("unchecked")
    public static <T extends Serializable> T deepCopy(T obj) {
        try {
            // Serialize to in-memory byte stream
            ByteArrayOutputStream bos = new ByteArrayOutputStream();
            ObjectOutputStream out = new ObjectOutputStream(bos);
            out.writeObject(obj);
            out.flush();

            // Deserialize from byte stream — produces a completely new object graph
            ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
            ObjectInputStream in = new ObjectInputStream(bis);
            return (T) in.readObject();
        } catch (IOException | ClassNotFoundException e) {
            throw new RuntimeException("Deep copy failed", e);
        }
    }
}

// All classes in the graph must implement Serializable
public class Employee implements Serializable {
    private static final long serialVersionUID = 1L;
    private int id;
    private String name;
    private Address address;
    // ...
}

public class Address implements Serializable {
    private static final long serialVersionUID = 1L;
    public String city;
    // ...
}

// Usage:
Employee original = new Employee(1, "Ravi", new Address("Mumbai"));
Employee clone = DeepCopyUtil.deepCopy(original);

clone.address.city = "Pune";
System.out.println(original.address.city); // "Mumbai" — fully independent
```

**Pros:** Works automatically on arbitrarily deep object graphs. No manual clone logic needed.  
**Cons:** Slower than manual copying. All classes in the graph must implement `Serializable`. Breaks if there are non-serializable fields (sockets, threads, etc.). Same `readResolve()` consideration as the Singleton serialization problem.

---

## Real-world Example — Game Character System

Let's build a complete example that combines the registry with multiple character types:

```java
// Prototype interface
public interface CharacterPrototype {
    CharacterPrototype clone();
    String describe();
}

// Character types
public class Warrior implements CharacterPrototype {
    private String name;
    private int hp, armor, strength;
    private List<String> abilities;

    public Warrior(String name, int hp, int armor, int strength, List<String> abilities) {
        this.name = name;
        this.hp = hp;
        this.armor = armor;
        this.strength = strength;
        this.abilities = abilities;
    }

    private Warrior(Warrior o) {
        this(o.name, o.hp, o.armor, o.strength, new ArrayList<>(o.abilities));
    }

    @Override public Warrior clone() { return new Warrior(this); }

    public Warrior withName(String name) { this.name = name; return this; }
    public Warrior withHp(int hp) { this.hp = hp; return this; }

    @Override
    public String describe() {
        return String.format("[Warrior] %s — HP:%d ARM:%d STR:%d %s",
            name, hp, armor, strength, abilities);
    }
}

public class Mage implements CharacterPrototype {
    private String name;
    private int hp, mana, spellPower;
    private List<String> spells;

    public Mage(String name, int hp, int mana, int spellPower, List<String> spells) {
        this.name = name;
        this.hp = hp;
        this.mana = mana;
        this.spellPower = spellPower;
        this.spells = spells;
    }

    private Mage(Mage o) {
        this(o.name, o.hp, o.mana, o.spellPower, new ArrayList<>(o.spells));
    }

    @Override public Mage clone() { return new Mage(this); }

    public Mage withName(String name) { this.name = name; return this; }

    @Override
    public String describe() {
        return String.format("[Mage] %s — HP:%d MP:%d SP:%d %s",
            name, hp, mana, spellPower, spells);
    }
}

// Registry
public class CharacterRegistry {
    private static final Map<String, CharacterPrototype> REGISTRY = Map.of(
        "warrior", new Warrior("Base Warrior", 200, 50, 80,
                      List.of("Slash", "Shield Bash", "Taunt")),
        "mage",    new Mage("Base Mage", 100, 300, 150,
                      List.of("Fireball", "Blizzard", "Teleport")),
        "warrior_elite", new Warrior("Elite Warrior", 350, 90, 120,
                      List.of("Whirlwind", "Berserker Rage", "Execute"))
    );

    public static CharacterPrototype create(String type) {
        CharacterPrototype proto = REGISTRY.get(type);
        if (proto == null) throw new IllegalArgumentException("Unknown type: " + type);
        return proto.clone();
    }
}

// Client code — no 'new Warrior(...)' anywhere
public class Main {
    public static void main(String[] args) {
        Warrior p1 = (Warrior) CharacterRegistry.create("warrior");
        p1.withName("Arjun").withHp(180);  // customise

        Warrior p2 = (Warrior) CharacterRegistry.create("warrior");
        p2.withName("Vikram");  // fresh clone, unaffected by p1

        Mage p3 = (Mage) CharacterRegistry.create("mage");
        p3.withName("Priya");

        System.out.println(p1.describe());
        // [Warrior] Arjun — HP:180 ARM:50 STR:80 [Slash, Shield Bash, Taunt]

        System.out.println(p2.describe());
        // [Warrior] Vikram — HP:200 ARM:50 STR:80 [Slash, Shield Bash, Taunt]

        System.out.println(p3.describe());
        // [Mage] Priya — HP:100 MP:300 SP:150 [Fireball, Blizzard, Teleport]
    }
}
```

---

## clone() with Nested Objects — Does it Work?

A common question: if `clone()` just delegates to the copy constructor like this —

```java
@Override
public GameCharacter clone() {
    return new GameCharacter(this);  // calls copy constructor
}
```

— will it correctly handle nested objects?

**Yes, but only if the copy constructor handles them correctly.** The `clone()` method itself is a thin wrapper. It does nothing more than call `new GameCharacter(this)`. The real deep-copy logic lives entirely inside the copy constructor. The two rise and fall together.

```
clone()  ──calls──>  copy constructor  ──decides fate of──>  each field
(wrapper)            (all the logic)
```

### Three scenarios

#### Scenario A — only primitives and Strings ✅ safe

```java
public class GameCharacter implements Prototype {
    private String role;      // String — immutable, safe to share reference
    private int health;       // primitive — copied by value
    private int attackPower;  // primitive — copied by value

    private GameCharacter(GameCharacter other) {
        this.role = other.role;               // safe — String is immutable
        this.health = other.health;           // safe — primitive
        this.attackPower = other.attackPower; // safe — primitive
    }

    @Override
    public GameCharacter clone() {
        return new GameCharacter(this);  // ✅ fully independent
    }
}
```

`String` is immutable — no one can change the characters it holds through the clone, so sharing the reference is harmless. Primitives are always value-copied. This works perfectly.

#### Scenario B — mutable nested object, copy constructor is careless ❌ broken

```java
public class GameCharacter implements Prototype {
    private String role;
    private int health;
    private Inventory inventory;   // mutable — danger zone

    // WRONG: copy constructor only copies the reference
    private GameCharacter(GameCharacter other) {
        this.role = other.role;
        this.health = other.health;
        this.inventory = other.inventory;  // ← just copies reference, not the object!
    }

    @Override
    public GameCharacter clone() {
        return new GameCharacter(this);  // calls the broken copy constructor
    }
}

// Problem:
GameCharacter original = registry.get("warrior");
GameCharacter clone = original.clone();

clone.inventory.addItem("Sword");  // mutates BOTH inventories!
System.out.println(original.inventory); // has "Sword" — broken
```

Here `clone()` is not to blame — it faithfully called the copy constructor. The bug is inside the copy constructor.

#### Scenario C — mutable nested object, copy constructor is correct ✅ fixed

```java
public class Inventory {
    private List<String> items;

    public Inventory(List<String> items) {
        this.items = new ArrayList<>(items);
    }

    // Inventory has its own copy constructor
    public Inventory(Inventory other) {
        this.items = new ArrayList<>(other.items);  // own list
    }

    public void addItem(String item) { this.items.add(item); }

    @Override
    public String toString() { return items.toString(); }
}

public class GameCharacter implements Prototype {
    private String role;
    private int health;
    private Inventory inventory;
    private List<String> skills;

    private GameCharacter(GameCharacter other) {
        this.role = other.role;                          // safe — immutable
        this.health = other.health;                      // safe — primitive
        this.inventory = new Inventory(other.inventory); // deep copy ✅
        this.skills = new ArrayList<>(other.skills);     // deep copy ✅
    }

    @Override
    public GameCharacter clone() {
        return new GameCharacter(this);  // ✅ now truly independent
    }
}

// Proof:
GameCharacter original = new GameCharacter("Warrior", 200,
    new Inventory(List.of("Shield")), new ArrayList<>(List.of("Slash")));

GameCharacter clone = original.clone();

clone.inventory.addItem("Sword");
clone.skills.add("Parry");

System.out.println(original.inventory); // [Shield]          ← unchanged
System.out.println(original.skills);    // [Slash]           ← unchanged
System.out.println(clone.inventory);    // [Shield, Sword]   ← independent
System.out.println(clone.skills);       // [Slash, Parry]    ← independent
```

### The rule for every field in the copy constructor

|Field type|What to do|Why|
|---|---|---|
|`int`, `boolean`, `double`, etc.|Copy directly: `this.x = other.x`|Primitives are value types — always independent|
|`String`|Copy directly: `this.name = other.name`|Immutable — no one can mutate the characters through a reference|
|`List`, `Map`, `Set`|`new ArrayList<>(other.list)`|Mutable — must own a separate collection|
|Custom mutable object|`new Address(other.address)` via its copy constructor|Mutable — must own a separate instance|
|Custom immutable object|Copy directly (same as String)|Immutable objects are inherently safe to share|

> **The key takeaway:** `clone()` returning `new GameCharacter(this)` works perfectly for nested objects — as long as the copy constructor uses `new` for every mutable field. `clone()` is never the bug; the copy constructor is where correctness is decided.

---

## Common Pitfalls

### Forgetting to deep copy mutable fields

The most common bug. Every `List`, `Map`, `Set`, `Date`, or custom mutable object inside the prototype **must** be explicitly cloned.

```java
// WRONG — skills list is shared
public Employee clone() {
    Employee copy = (Employee) super.clone();
    // forgot to clone skills!
    return copy;
}

// CORRECT
public Employee clone() {
    Employee copy = (Employee) super.clone();
    copy.skills = new ArrayList<>(this.skills);   // own copy
    copy.address = new Address(this.address);      // own copy
    return copy;
}
```

### Returning the original from the registry

The registry must always return a clone. Returning the stored prototype directly means all clients share the same object — the whole point is defeated.

```java
// WRONG
public Prototype get(String key) {
    return registry.get(key);  // returns the actual prototype — no clone!
}

// CORRECT
public Prototype get(String key) {
    return registry.get(key).clone();  // always clone
}
```

### Circular references in deep copy

If objects have circular references (A holds reference to B, B holds reference back to A), naive recursive deep copy will cause a `StackOverflowError`. Use serialization-based copy or a `IdentityHashMap` visited set for manual deep copy in such cases.

### `Cloneable` without `implements Cloneable`

If you call `super.clone()` but the class does not implement `Cloneable`, you get `CloneNotSupportedException` at runtime even though your code compiles fine. Always pair `super.clone()` with `implements Cloneable`.

---

## Prototype vs Factory

||Prototype|Factory Method / Abstract Factory|
|---|---|---|
|How objects are created|Clone existing instance|Build from scratch|
|Use when|You have a pre-configured base object|You need a fresh object each time|
|Configuration|Carried by the prototype|Applied during construction|
|Performance|Faster (copy vs full initialization)|Slower if initialization is heavy|
|Subclass knowledge|Client needs no knowledge of concrete type|Often needs to know which factory to call|
|Variation mechanism|Clone and mutate|Subclass or parameterize the factory|

**Choose Prototype when:**

- The cost of `new` is high (DB queries, file I/O, network)
- You want to offer "template" objects that users configure
- You have a combinatorial number of configurations (prefer registry over subclasses)

**Choose Factory when:**

- Every object should start fresh with no carried state
- Object construction logic is complex but the objects themselves are lightweight

---

## Quick Summary

|Approach|Deep copy?|Complexity|Best for|
|---|---|---|---|
|`Cloneable` shallow|❌|Low|Primitives + immutable fields only|
|`Cloneable` deep|✅|Medium|Simple object graphs|
|Copy constructor|✅|Medium|Most cases — cleanest API|
|Registry + clone|✅|Medium|Multiple named templates, decoupled clients|
|Serialization|✅ (automatic)|Low code, slow runtime|Complex/deep object graphs with `Serializable` classes|

### Key takeaways

- **Always deep copy mutable fields** — shared references between original and clone cause subtle, hard-to-find bugs.
- **The copy constructor approach is cleanest** — no `Cloneable` baggage, no checked exceptions, fully explicit.
- **The registry pattern is the most practical** in real applications — clients never need to know concrete types or call `new`.
- **Serialization copy is a pragmatic fallback** for complex object graphs, at the cost of runtime performance.