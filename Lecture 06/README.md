# Lecture 06: SOLID Principles (Part 2 - I, D) & Formal LSP Rules

---

## 🎯 Concepts at a Glance

SOLID ke aakhiri do principles aur Liskov Substitution Principle (LSP) ke mathematical/formal rules:
1. **ISP (Interface Segregation Principle)**: "Clients should not be forced to depend on interfaces they do not use." (Fat interfaces ko todkar lean interfaces banao).
2. **DIP (Dependency Inversion Principle)**: "High-level modules should not depend on low-level modules. Both should depend on abstractions."
3. **Formal LSP Rules**: Preconditions, Postconditions, Invariants, and Exception rules.

---

## 1. 🍽️ Interface Segregation Principle (ISP)

> **Golden Rule**: *"Do not force a class to implement methods it doesn't need. Prefer many small, client-specific interfaces over one bulky/fat interface."*

### 💡 Real-World Analogy: Multipurpose Tool vs Focused Tools
Socho aap ek Restaurant mein gaye aur unhone aapko 50-page ka menu de diya jisme Indian, Chinese, Italian, Mexican, Automotive Repair, aur Laundry services sab likhi hain!
Aapko sirf khana khana hai, lekin aapko laundry service ka option dekhna pad raha hai.
Programming mein bhi yahi hota hai: jab ek interface sab kuch offer karta hai, toh jo classes uske aadhe methods use nahi karti, unhe zabardasti dummy/blank implementations likhni padti hain.

### ❌ Violation (Fat Interface)
```java
interface Shape {
    double area();
    double volume(); // 2D shapes (Square, Circle) ke paas volume nahi hota!
}

class Square implements Shape {
    public double area() { return side * side; }
    public double volume() {
        // Square ka koi volume nahi hota!
        throw new UnsupportedOperationException("Square has no volume!");
    }
}
```

### ✅ Followed (Lean Interfaces)
```java
interface TwoDimensionalShape {
    double area();
}

interface ThreeDimensionalShape {
    double area();
    double volume();
}

class Square implements TwoDimensionalShape { ... }
class Cube implements ThreeDimensionalShape { ... }
```

---

## 2. 🔌 Dependency Inversion Principle (DIP)

> **Golden Rule**:
> 1. High-level modules (business logic) should not depend on low-level modules (DB, UI, Third-party APIs). Both should depend on **Abstractions**.
> 2. Abstractions should not depend on details. Details should depend on abstractions.

### 💡 Real-World Analogy: Wall Electrical Socket
Aapke ghar ki electrical wall socket ek **Abstraction (Interface)** hai jo 230V AC provide karti hai.
- Aapka TV (High-level) directly wall ke andar ke wire se permanently soldered nahi hota.
- TV bhi 2-pin/3-pin plug (Interface) use karta hai aur Socket bhi wahi standard follow karta hai.
- Kal ko aap TV hata kar Fridge ya Laptop Charger plug kar sakte ho. Dono ek dusre se decoupled hain!

### ❌ Violation (Tight Coupling)
```java
class UserService {
    // Direct dependency on concrete low-level MySQL class!
    private MySQLDatabase db = new MySQLDatabase();

    public void storeUser(String user) {
        db.saveToSQL(user);
    }
}
```
*Problem*: Agar kal ko MongoDB use karna pade, toh `UserService` ki core business logic modify karni padegi.

### ✅ Followed (Dependency Injection via Interface)
```java
// Abstraction
interface Database {
    void save(String data);
}

// Low-level implementations
class MySQLDatabase implements Database { ... }
class MongoDBDatabase implements Database { ... }

// High-level module depends ONLY on abstraction
class UserService {
    private final Database db;

    // Injected from outside (Constructor Injection)
    public UserService(Database db) {
        this.db = db;
    }

    public void storeUser(String user) {
        db.save(user);
    }
}
```

---

## 3. 🔬 Deep-Dive: Formal Rules of LSP (Interview Special)

Barbara Liskov ke original paper ke hisaab se LSP ke 3 categories of rules hote hain:

### A. Method Rules (Pre & Post Conditions)
1. **Preconditions cannot be strengthened in subtype**:
   - Agar Parent class method kehti hai: *"Password at least 6 characters ka hona chahiye"*, toh Subclass ye nahi bol sakti ki *"Mujhe at least 10 characters chahiye"*. Client base type samajhkar 7 characters dega aur code fail ho jayega! Subclass precondition ko weaken kar sakti hai (e.g. 4 characters allow kare), lekin strengthen nahi.
2. **Postconditions cannot be weakened in subtype**:
   - Agar Parent class guarantee karti hai ki *"Return value always > 0 hogi"*, toh Subclass negative number return nahi kar sakti. Subclass guarantee ko strong kar sakti hai, kamzor nahi.

### B. Property Rules (Invariants & History)
1. **Class Invariants**:
   - Parent class ke jo rules/conditions hamesha true rehte hain (e.g., `balance >= 0`), Subclass un rules ko break nahi kar sakti.
2. **History Constraint**:
   - Agar Parent class immutable hai (e.g., state change karne ka koi method nahi hai), toh Subclass me setter bana kar state mutable nahi banayi ja sakti.

### C. Signature Rules
1. **Return Type Covariance**: Subclass method same ya zyada specific (child) return type return kar sakti hai.
2. **Exception Rule**: Subclass method parent method se naye ya zyada broad checked exceptions throw nahi kar sakti.

---

## 📝 Lecture 06 Summary (Quick Revision)

- **ISP**: Interfaces ko chota aur purpose-driven rakho taaki client ko unchahi methods implement na karni padein.
- **DIP**: High-level logic ko directly DB ya 3rd-party concrete classes se bind mat karo; dono ke beech Interface lagao (Dependency Injection).
- **LSP Formal Rules**: Subclass parent ke promises (postconditions, invariants) ko tod nahi sakti aur parent se zyada demands (preconditions) nahi kar sakti.
