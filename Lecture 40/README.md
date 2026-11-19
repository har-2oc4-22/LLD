# Lecture 40: Low-Level Design Wrap-Up & Machine Coding Interview Strategy

---

## 🎯 The Ultimate LLD Machine Coding Interview Framework

LLD / Machine Coding rounds (45 to 90 minutes) mein interviewer aapki coding speed, object-oriented modeling skills, clean code conventions, aur design pattern selection ko evaluate karta hai.

---

## 🗺️ The 5-Step Problem Solving Framework

```text
[Step 1: Clarify Requirements] ──> [Step 2: Identify Entities] ──> [Step 3: Relationships & UML]
                                                                             │
[Step 5: Code & Validate] <── [Step 4: Choose Design Patterns] <─────────────┘
```

### 1. 📋 Step 1: Requirement Clarification (5–10 mins)
- Problem statement sunte hi code likhna start **mat karo**.
- **Functional Requirements**: System ko kya-kya karna chahiye? (e.g., Splitwise mein Equal split, Exact split, Percentage split support hona chahiye).
- **Non-Functional Boundaries**: Single-machine vs multi-threaded, in-memory data structures vs DB persistence.
- Scope out things: Interviewer se explicitly confirm karo ki kya cheez skip ki ja sakti hai (e.g., authentication ya frontend).

### 2. 🧱 Step 2: Identify Entities & Models (5 mins)
- Problem description mein se **Nouns (Sangya)** dhoondo:
  - Example (Food Delivery App): `User`, `Restaurant`, `MenuItem`, `Cart`, `Order`, `Payment`.
- Har entity ke attributes (fields) aur behaviors (methods) list karo.

### 3. 📐 Step 3: Define Class Relationships (5–10 mins)
- Kon kisko hold karega?
  - `Order` HAS-A `Cart` (Composition).
  - `Restaurant` HAS-A `List<MenuItem>` (Aggregation).
  - `Payment` IS-A `PaymentStrategy` (Strategy / Interface).

### 4. 🎨 Step 4: Pick the Right Design Patterns (Never Over-Engineer!)
Patterns tabhi use karo jab unki genuine need ho:
- Multiple algorithms with same goal? ➡️ **Strategy Pattern** (e.g. Payment, Sorting, Route).
- Notification / Event triggering? ➡️ **Observer Pattern** (e.g. Order status updates).
- Complex object creation with many options? ➡️ **Builder Pattern** (e.g. Filter queries, HTTP requests).
- Object changes behavior based on state? ➡️ **State Pattern** (e.g. Vending Machine, Order Lifecycle).
- Wrapping / Decorating features? ➡️ **Decorator Pattern** (e.g. Pizza toppings, Coffee addons).

### 5. 💻 Step 5: Implementation & Clean Code (30–45 mins)
- Follow **SOLID Principles**:
  - Keep classes small and focused (SRP).
  - Use interfaces for pluggability (OCP & DIP).
- Write modular code: `models/`, `strategies/`, `factories/`, `managers/`.
- Handle edge cases: Null pointer checks, invalid inputs, insufficient balance.
- Write a clear `main()` method with end-to-end happy path and edge-case execution.

---

## 📚 Complete Design Patterns Quick Cheat-Sheet

| Pattern | Category | When to Use in LLD Rounds |
| :--- | :--- | :--- |
| **Singleton** | Creational | Connection pool, App Config, In-memory cache |
| **Factory / Abstract Factory** | Creational | Decouple object creation based on dynamic inputs |
| **Builder** | Creational | Objects with 5+ parameters or step-by-step configuration |
| **Prototype** | Creational | Creating copies of expensive/heavy template objects |
| **Adapter** | Structural | Connecting two incompatible interfaces (Legacy vs Modern) |
| **Decorator** | Structural | Adding runtime dynamic features (avoiding class explosion) |
| **Facade** | Structural | One simple front-door interface for a complex multi-class subsystem |
| **Composite** | Structural | Part-whole tree hierarchies (File system, Org charts) |
| **Proxy** | Structural | Access control, lazy loading, caching |
| **Bridge** | Structural | Decoupling Abstraction from Implementation ($M \times N \to M+N$) |
| **Flyweight** | Structural | Memory optimization for massive numbers of duplicate objects |
| **Strategy** | Behavioral | Swapping interchangeable algorithms at runtime |
| **Observer** | Behavioral | One-to-many event notification (Pub-Sub) |
| **Command** | Behavioral | Encapsulating requests as objects (Undo/Redo, queues) |
| **State** | Behavioral | State machine with distinct behavior per state |
| **Chain of Responsibility** | Behavioral | Sequential pipelines, middlewares, ATM note dispensers |
| **Template Method** | Behavioral | Fixed algorithm skeleton with customizable subclass steps |
| **Iterator** | Behavioral | Uniform collection traversal without exposing internal structure |
| **Mediator** | Behavioral | Eliminating $N^2$ direct mesh dependencies between colleagues |
| **Visitor** | Behavioral | Adding new operations to stable classes via Double Dispatch |
| **Memento** | Behavioral | Transaction rollback, save/restore state snapshots |

---

## 📝 Lecture 40 Summary (Quick Revision)

- **Clarity over Complexity**: Simple, working, readable code wins over complicated half-baked designs.
- **Drive with SOLID**: Every design decision should be justified by SRP, OCP, or DIP.
- **Test in `main()`**: Always provide a working executable demonstration in your machine coding rounds.
