# Lecture 25: Bridge Design Pattern

---

## 🎯 What is Bridge Pattern?

> **Definition**: *"Decouple an abstraction from its implementation so that the two can vary independently."*

Jab aapke paas do independent dimensions (variations) hote hain jo sath me grow karte hain (e.g. Car Types aur Engine Types), tab Bridge pattern un dono dimensions ko alag-alag hierarchies mein baant deta hai aur unke beech ek **Bridge (HAS-A composition)** bana deta hai.

---

## 💡 The Problem: The $M \times N$ Cartesian Explosion

Maan lo aapko Car models model karne hain:
- **Car Types (Abstraction)**: `Sedan`, `SUV`, `Hatchback` (3 types)
- **Engine Types (Implementation)**: `PetrolEngine`, `DieselEngine`, `ElectricEngine` (3 types)

### ❌ If using pure Inheritance:
Aapko har combination ki nayi class banani padegi:
- `PetrolSedan`, `DieselSedan`, `ElectricSedan`
- `PetrolSUV`, `DieselSUV`, `ElectricSUV`
- `PetrolHatchback`, `DieselHatchback`, `ElectricHatchback`
👉 **Total Classes = $3 \times 3 = 9$ classes!**
Agar kal ko `HybridEngine` aur `SportsCar` add ho gaya, toh classes badhkar **$4 \times 4 = 16$** ho jayengi!

---

## ✅ The Bridge Solution: $M + N$ instead of $M \times N$

Dono dimensions ko alag-alag split karke bridge banao:
1. **Abstraction**: `Car` -> `Sedan`, `SUV` (2 classes)
2. **Implementation**: `Engine` -> `Petrol`, `Diesel`, `Electric` (3 classes)
3. **Bridge**: `Car` ke andar `Engine` reference inject hota hai!
👉 **Total Classes = $2 + 3 = 5$ classes only!**

```text
[Abstraction Hierarchy]                      [Implementor Hierarchy]
      [Car] ────────────── HAS-A (Bridge) ─────────────> [Engine]
      /   \                                             /    │    \
 [Sedan]  [SUV]                                   [Petrol] [Diesel] [Electric]
```

---

## 💻 Code Walkthrough

### 1. Implementor Hierarchy (Engine)
```java
interface Engine {
    void start();
}

class PetrolEngine implements Engine {
    public void start() { System.out.println("Petrol engine starting with ignition!"); }
}

class ElectricEngine implements Engine {
    public void start() { System.out.println("Electric engine powering up silently!"); }
}
```

### 2. Abstraction Hierarchy (Car holding Engine bridge)
```java
abstract class Car {
    protected Engine engine; // The Bridge!

    public Car(Engine e) { this.engine = e; }
    public abstract void drive();
}

class Sedan extends Car {
    public Sedan(Engine e) { super(e); }
    public void drive() {
        engine.start();
        System.out.println("Driving a Sedan on the highway.");
    }
}

class SUV extends Car {
    public SUV(Engine e) { super(e); }
    public void drive() {
        engine.start();
        System.out.println("Driving an SUV off-road.");
    }
}
```

### 3. Client Usage: Mix and Match Freely
```java
Engine electric = new ElectricEngine();
Car teslaSUV = new SUV(electric); // Combine any car with any engine!
teslaSUV.drive();
```

---

## ⚖️ Bridge vs Adapter

| Parameter | Bridge Pattern | Adapter Pattern |
| :--- | :--- | :--- |
| **When used** | **Design phase** par pehle se architecture clean rakhne ke liye | **Post-design / Legacy integration** mein do mismatched classes ko jodne ke liye |
| **Intent** | Abstraction aur Implementation ko independently grow karwana | Do existing incompatible interfaces ko compatible banana |

---

## 📝 Lecture 25 Summary (Quick Revision)

- **Prevents Class Explosion**: $M \times N$ classes ko $M + N$ classes mein reduce karta hai.
- **Two Dimensions of Variance**: Jab feature aur platform/backend dono independently change ho sakte hon (e.g. `Notification` (SMS/Email) on `Platform` (iOS/Android)).
- **Loose Coupling**: Abstraction implementation ke internal details se unaware rehti hai.
