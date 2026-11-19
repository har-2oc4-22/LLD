# Low-Level Design (LLD) & Object-Oriented Design

A comprehensive repository containing Low-Level Design (LLD) principles, Design Patterns, and Machine Coding solutions implemented in both **Java** and **C++**, complete with architecture notes and UML diagrams.

---

## 📚 Table of Contents

- [🎯 Basic Prerequisites (Must Know Before LLD)](#-basic-prerequisites-must-know-before-lld)
- [Core OOP Concepts](#-core-oop-concepts)
- [SOLID Principles](#-solid-principles)
- [Design Patterns](#-design-patterns)
  - [Creational Patterns](#creational-patterns)
  - [Structural Patterns](#structural-patterns)
  - [Behavioral Patterns](#behavioral-patterns)
- [Real-World Case Studies & Machine Coding](#-real-world-case-studies--machine-coding)
- [Practice Problems & Hackathons](#-practice-problems--hackathons)
- [How to Run](#-how-to-run)

---

## 🎯 Basic Prerequisites (Must Know Before LLD)

Low-Level Design (LLD) aur Machine Coding mein enter karne se pehle in 5 core building blocks ki strong understanding hona zaroori hai:

### 1. 💻 Language & OOP Fundamentals (Java or C++)
- **Classes, Objects & Lifecycle**: Memory allocation, constructors, copy constructors, destructors.
- **Access Modifiers & Encapsulation**: Proper usage of `private` (data hiding), `protected` (inheritance), aur `public`.
- **Abstract Classes vs Interfaces**:
  - *Interface*: Pure contract / capabilities (`PaymentStrategy`, `Flyable`, `Observer`). Multiple interfaces implement ho sakte hain.
  - *Abstract Class*: Partial implementation + code sharing (`Vehicle`, `Robot`, `ModelTrainer`).
- **Enums & Constants**: Magic strings (`"PENDING"`, `"SUCCESS"`) avoid karke strongly-typed enums (`OrderStatus.PENDING`, `DeviceType.BLUETOOTH`) use karna.
- **Generics / Templates**: Reusable type-safe structures (`Iterator<T>`, `Pair<K, V>`).

### 2. 🗃️ Essential Data Structures for Machine Coding
LLD mein database ki jagah in-memory collections use hoti hain:
- **`Map<K, V>` / `HashMap`**: Entity lookup by ID, indexing, state tracking ($O(1)$ lookups).
- **`List<T>` / `ArrayList`**: Maintaining ordered items, shopping carts, event history.
- **`Deque<T>` / `Queue`**: Round-robin player turn rotation (e.g. Tic-Tac-Toe, Snake & Ladder) aur background task execution.
- **`Stack<T>`**: Undo / Redo histories, Inorder tree iterators, rollback operations.
- **`Set<T>`**: Tracking unique visitors, muting users, graph cycle detection.

### 3. 📐 UML & Class Relationship Notations
Class diagrams ko read aur draw karne ke liye ye 6 symbols clear hone chahiye:
```text
Dependency:      [Driver]  - - - - >  [Car]           (Uses-a: method parameter)
Association:     [Doctor]  ───────  [Patient]         (Knows-a: structural peer link)
Aggregation:     [Dept]    ◇──────  [Professor]       (Has-a: weak link, independent life)
Composition:     [House]   ◆──────  [Room]            (Has-a: strong link, death dependency)
Generalization:  [Dog]     ──────▷  [Animal]          (Is-a: inheritance)
Realization:     [CreditCard] - - ▷ [PaymentStrategy] (Implements: interface)
```

### 4. 🔒 Concurrency & Thread-Safety Basics
High-scale machine coding systems (e.g. Coupon Redemption, Singleton, Inventory) mein multi-threading concepts aate hain:
- **Race Condition**: Jab multiple threads bina synchronization ke shared state ko simultaneously modify karte hain.
- **`synchronized` blocks & `ReentrantLock`**: Critical sections ko protect karne ke liye locking.
- **`volatile` keyword**: Instruction reordering rokna aur thread memory visibility ensure karna (e.g. Double-Checked Locking Singleton).
- **Atomic Variables**: `AtomicInteger` lock-free thread-safe counters ke liye.

### 5. 🧠 The LLD Mindset & Clean Code Heuristics
- **Program to an Interface, not an Implementation**: Hamesha base interface reference hold karo (`List<Product> list = new ArrayList<>()`), concrete class se tightly couple mat bano.
- **Favor Composition over Inheritance**: Code reuse ke liye "HAS-A" relationship "IS-A" se zyada flexible aur maintainable hoti hai.
- **Separation of Concerns (SoC)**:
  - `models/`: Pure state containers (Data classes).
  - `managers/` ya `services/`: Business workflows and operations.
  - `strategies/`: Interchangeable algorithms.
  - `factories/`: Object creation logic.
- **DRY (Don't Repeat Yourself)** & **YAGNI (You Aren't Gonna Need It)**: Over-engineering mat karo, wahi likho jo requirements demand karein.

---

## 🏛️ Core OOP Concepts

| Topic | Description | Notes / Code |
| :--- | :--- | :--- |
| **Lecture 01** | Introduction to Object-Oriented Programming & Low-Level Design | [Notes](./Lecture%2001/Notes.pdf) |
| **Lecture 02** | Abstraction & Encapsulation | [Java](./Lecture%2002/Java%20Code) \| [C++](./Lecture%2002/C%2B%2B%20Code) \| [Notes](./Lecture%2002/Notes.pdf) |
| **Lecture 03** | Inheritance & Polymorphism (Static vs Dynamic) | [Java](./Lecture%2003/Java%20Code) \| [C++](./Lecture%2003/C%2B%2B%20Code) \| [Notes](./Lecture%2003/notes.pdf) |
| **Lecture 04** | Object Modeling & Class Relationships | [Notes](./Lecture%2004/notes.pdf) |

---

## ⚖️ SOLID Principles

| Principle | Topic | Reference |
| :--- | :--- | :--- |
| **SRP** | Single Responsibility Principle | [Lecture 05](./Lecture%2005) |
| **OCP** | Open / Closed Principle | [Lecture 05](./Lecture%2005) |
| **LSP** | Liskov Substitution Principle & Substitution Rules | [Lecture 05](./Lecture%2005), [Lecture 06](./Lecture%2006) |
| **ISP** | Interface Segregation Principle | [Lecture 06](./Lecture%2006) |
| **DIP** | Dependency Inversion Principle | [Lecture 06](./Lecture%2006) |

---

## 🎨 Design Patterns

### Creational Patterns
- **Factory & Abstract Factory Pattern** - [Lecture 09](./Lecture%2009)
- **Singleton Pattern (Thread-safe, Lazy & Eager implementations)** - [Lecture 10](./Lecture%2010)
- **Builder Pattern (Simple, Step Builder, Director)** - [Lecture 28](./Lecture%2028)
- **Prototype Pattern** - [Lecture 36](./Lecture%2036)

### Structural Patterns
- **Decorator Pattern** - [Lecture 13](./Lecture%2013)
- **Adapter Pattern** - [Lecture 16](./Lecture%2016)
- **Facade Pattern** - [Lecture 17](./Lecture%2017)
- **Composite Pattern** - [Lecture 19](./Lecture%2019)
- **Proxy Pattern (Virtual, Protection & Remote Proxy)** - [Lecture 21](./Lecture%2021)
- **Bridge Pattern** - [Lecture 25](./Lecture%2025)
- **Flyweight Pattern** - [Lecture 30](./Lecture%2030)

### Behavioral Patterns
- **Strategy Pattern** - [Lecture 08](./Lecture%2008)
- **Observer Pattern** - [Lecture 12](./Lecture%2012)
- **Command Pattern** - [Lecture 15](./Lecture%2015)
- **Template Method Pattern** - [Lecture 20](./Lecture%2020)
- **Chain of Responsibility Pattern** - [Lecture 22](./Lecture%2022)
- **Iterator Pattern** - [Lecture 29](./Lecture%2029)
- **State Pattern** - [Lecture 32](./Lecture%2032)
- **Mediator Pattern** - [Lecture 35](./Lecture%2035)
- **Visitor Pattern** - [Lecture 38](./Lecture%2038)
- **Memento Pattern** - [Lecture 39](./Lecture%2039)

---

## 🚀 Real-World Case Studies & Machine Coding

End-to-end design solutions with full source code and UML diagrams:

1. **Document Editor** ([Lecture 07](./Lecture%2007))
   - Comparison of naive design vs good design applying SOLID principles.
2. **Tomato (Food Delivery App)** ([Lecture 11](./Lecture%2011))
   - Order scheduling, restaurant management, cart handling, and multiple payment strategies.
3. **Notification Service** ([Lecture 14](./Lecture%2014))
   - Multi-channel notification pipeline (Email, SMS, Push).
4. **Music Player Application** ([Lecture 18](./Lecture%2018))
   - Audio output device adapters, playback strategies (random, sequential, queue), and facade interface.
5. **Payment Gateway Application** ([Lecture 23](./Lecture%2023))
   - Banking integration, routing, and payment verification.
6. **Coupon & Discount Engine** ([Lecture 24](./Lecture%2024))
   - Tiered discounts, concurrent coupon redemption, and thread-safe locking.
7. **Zepto Clone (Quick Commerce)** ([Lecture 26](./Lecture%2026))
   - Inventory management, warehouse allocation, and lightning delivery simulation.
8. **Tinder Clone (Matchmaking System)** ([Lecture 27](./Lecture%2027))
   - User profiling, preference-based matching algorithm, and interaction states.
9. **Splitwise (Expense Sharing System)** ([Lecture 31](./Lecture%2031))
   - Split algorithms (Exact, Equal, Percentage) with transaction simplification graph.
10. **Vending Machine** ([Lecture 32](./Lecture%2032))
    - State machine implementation for coin insertion, item selection, and dispensing.
11. **Tic-Tac-Toe** ([Lecture 33](./Lecture%2033))
    - Modular game loop, board state management, and win-condition evaluators.
12. **Snake & Ladder** ([Lecture 34](./Lecture%2034))
    - Dynamic board generation, dice rolling strategy, and multiplayer turn management.
13. **Chess Game** ([Lecture 37](./Lecture%2037))
    - Piece hierarchy, move validation, check/checkmate detection, and turn alternation.

---

## 💡 Practice Problems & Hackathons

- [Hackathon 01 Problem Statement](./Hackathon%2001/Problem%20Statement.pdf)
- [Practice Problem 1](./PracticeProblems/PracticeProblem1.pdf)
- [Practice Problem 2](./PracticeProblems/PracticeProblem2.pdf)
- [Lecture 40 Wrap-Up Notes](./Lecture%2040/Notes.pdf)

---

## 🛠️ How to Run

### Java
Ensure JDK (version 17 or higher) is installed:
```bash
# Compile
javac path/to/YourFile.java

# Run
java path/to/YourFile
```

### C++
Ensure a modern C++ compiler (`g++` or `clang++` supporting C++17/C++20) is installed:
```bash
# Compile
g++ -std=c++20 path/to/YourFile.cpp -o app

# Run
./app
```

