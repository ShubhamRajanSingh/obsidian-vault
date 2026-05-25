---
title: SDE-2 Low-Level Design (LLD) Master Plan
tags:
  - interview-prep
  - LLD
  - SDE2
  - system-design
---

# LLD Study & Practice Plan

This is a comprehensive study plan for mastering Low-Level Design (LLD) for SDE 2 interviews. It covers the core foundations, design patterns, and a tiered list of 22 practice problems.

## 📚 Phase 1: Core Foundations

### Object-Oriented Programming (OOP)
- [ ] **Encapsulation:** Data hiding and access modifiers.
- [ ] **Abstraction:** Exposing only necessary details (Interfaces vs. Abstract classes).
- [ ] **Inheritance:** "Is-a" relationships and their pitfalls.
- [ ] **Polymorphism:** Method overriding and overloading.
- [ ] **Composition over Inheritance:** "Has-a" vs "Is-a" relationships.

### SOLID Principles (The Bedrock)
- [ ] **S**ingle Responsibility Principle (SRP): One reason to change.
- [ ] **O**pen/Closed Principle (OCP): Open for extension, closed for modification.
- [ ] **L**iskov Substitution Principle (LSP): Subtypes must be substitutable.
- [ ] **I**nterface Segregation Principle (ISP): Client-specific interfaces.
- [ ] **D**ependency Inversion Principle (DIP): Depend on abstractions.

### UML Diagrams (Keep it practical)
- [ ] Class Diagrams (Entities, Attributes, Methods).
- [ ] Relationships: Association, Aggregation, Composition.
- [ ] Use Case Diagrams.
- [ ] Sequence Diagrams.

---

## 🛠️ Phase 2: Design Patterns

*Focus on the practical application of these patterns rather than rote memorization. Since you work across paradigms, pay attention to how patterns like Strategy or Command often map cleanly to higher-order functions or closures in functional/multiparadigm approaches.*

### Creational Patterns (Object Creation)
- [ ] **Factory Method:** Defer instantiation to subclasses.
- [ ] **Abstract Factory:** Families of related objects.
- [ ] **Builder:** Step-by-step creation of complex objects.
- [ ] **Singleton:** Ensuring a single instance (focus on Thread-Safe implementation).

### Structural Patterns (Object Assembly)
- [ ] **Adapter:** Making incompatible interfaces work together.
- [ ] **Decorator:** Adding responsibilities dynamically.
- [ ] **Facade:** A simplified interface to a complex subsystem.

### Behavioral Patterns (Object Communication)
- [ ] **Strategy:** Interchangeable algorithms (Crucial for LLD).
- [ ] **Observer:** Pub-Sub / Event listeners (Crucial for LLD).
- [ ] **State:** Alter behavior when internal state changes.
- [ ] **Command:** Encapsulate a request as an object.

---

## 💻 Phase 3: Practice Problems

Work through these incrementally. For each problem:
1. Define actors/use cases.
2. Identify core entities.
3. Draw a class diagram.
4. Write the core logic and interfaces.

### Level 1: Foundational (Entities & States)
These problems test basic OOP, SOLID, and patterns like State and Strategy.
- [ ] 1. Design a Parking Lot
- [ ] 2. Design a Vending Machine
- [ ] 3. Design a Tic-Tac-Toe Game
- [ ] 4. Design a Snake and Ladder Game
- [ ] 5. Design a Library Management System
- [ ] 6. Design a Vehicle Rental System
- [ ] 7. Design an ATM System

### Level 2: Intermediate (Concurrency, Rules, & Algorithms)
These are standard SDE-2 questions. They test how you handle concurrency, complex pricing, and graph/tree logic.
- [ ] 8. Design an Elevator System
- [ ] 9. Design a Movie Ticket Booking System (e.g., BookMyShow)
- [ ] 10. Design Splitwise (Expense sharing)
- [ ] 11. Design a Restaurant Management System (Swiggy / Zomato)
- [ ] 12. Design an In-Memory Cache (LRU / LFU Eviction policies)
- [ ] 13. Design an E-commerce Platform (Amazon checkout flow)
- [ ] 14. Design a Ride-Sharing Service (Uber pricing and matching)

### Level 3: Advanced / Infrastructure LLD
These blend LLD with core computer science concepts (networking, OS, data structures).
- [ ] 15. Design a Message Queue (Pub/Sub pattern, like Kafka/RabbitMQ lite)
- [ ] 16. Design a Task Scheduler / Cron Job Execution Framework
- [ ] 17. Design a Rate Limiter (Token Bucket / Leaky Bucket LLD)
- [ ] 18. Design an Online Chess Game (Complex rule validation)
- [ ] 19. Design a Meeting Scheduler (Calendly overlapping intervals)
- [ ] 20. Design a Payment Gateway (Idempotency and state handling)
- [ ] 21. Design a Logging Framework (e.g., Log4j - Chain of Responsibility pattern)
- [ ] 22. Design a Notification System (Email/SMS/Push strategies)

---

## 📋 The SDE-2 LLD Interview Checklist
*Keep this in mind during mock interviews or actual rounds.*

- [ ] **Did I clarify requirements?** (Never assume. Ask constraints).
- [ ] **Did I list out all the core entities/classes first?**
- [ ] **Is my design tightly coupled?** (Check for Dependency Injection opportunities).
- [ ] **Have I used Enums for fixed types (Status, VehicleType, etc.)?**
- [ ] **Did I isolate the core business logic from the driver/execution code?**
- [ ] **Are there concurrency issues?** (e.g., Booking the same seat, deducting balance).
- [ ] **Can my design accommodate a new requirement easily?** (The ultimate test of OCP).
