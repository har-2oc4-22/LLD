# Lecture 03: Inheritance & Polymorphism

---

## 🎯 Concepts at a Glance

OOP ke baaki do pillars:
1. **Inheritance**: Code reusability & hierarchical classification (**IS-A Relationship**).
2. **Polymorphism**: "Ek naam, anek roop" (One interface, multiple implementations).
   - **Static (Compile-Time)**: Method Overloading.
   - **Dynamic (Run-Time)**: Method Overriding.

---

## 1. 🧬 Inheritance Explained

### 💡 Real-World Analogy
Socho ek `Vehicle` family hai:
- Sabhi vehicles ke paas `brand`, `model`, `startEngine()`, aur `stopEngine()` common hota hai.
- Lekin `ElectricCar` ke paas battery hoti hai (`chargeBattery()`), jabki `ManualCar` ke paas clutch aur gear stick hoti hai (`shiftGear()`).
- Har car ke liye common engine code dobara likhne ki bajaye hum ek base class `Car` banate hain aur `ManualCar` aur `ElectricCar` usse inherit kar lete hain.

```text
         [Car] (Base Class: brand, model, startEngine)
          /  \
         /    \
 [ManualCar]  [ElectricCar]
```

> **Relationship Rule**: Agar "X is a Y" true hai (e.g., *ElectricCar is a Car*), tabhi inheritance use karo. Agar "X has a Y" hai (e.g., *Car has an Engine*), toh composition use karo.

---

## 2. 🎭 Polymorphism Explained

Polymorphism ka matlab hota hai **"Many Forms"**. Same function call alag-alag behavior deti hai situation ke hisaab se.

### A. Static (Compile-time) Polymorphism: Method Overloading
Jab ek hi class ke andar ek hi naam ke multiple methods hote hain, lekin unke **parameters (count ya type)** alag hote hain.

```java
// Example: Same method name, different arguments
public void accelerate();             // Default speed jump (+20)
public void accelerate(int speed);    // Custom speed jump (+speed)
```
- **Faisla kab hota hai?** Compile time par, compiler dekh leta hai ki aapne kitne arguments pass kiye hain.

---

### B. Dynamic (Run-time) Polymorphism: Method Overriding
Jab Child class Parent class ke method ko apne hisaab se re-define karti hai.

```java
Car myCar = new ElectricCar("Tesla", "Model S");
myCar.accelerate(); 
// At runtime, JVM decides: myCar actually points to ElectricCar, 
// so ElectricCar's accelerate() with battery depletion will run!
```

| Type | How it Works | Example | Decided At |
| :--- | :--- | :--- | :--- |
| **Overloading** | Same name, different parameter signature | `accelerate()` vs `accelerate(50)` | **Compile-Time** |
| **Overriding** | Child class alters parent class method logic | `ElectricCar.brake()` using regenerative braking | **Run-Time** |

---

## 💻 Code Example from Lecture

```java
// Base class reference pointing to derived class objects
Car manual = new ManualCar("Ford", "Mustang");
Car ev = new ElectricCar("Tesla", "Model S");

manual.accelerate(); // Normal fuel acceleration (+20 km/h)
ev.accelerate();     // Battery consumption (-10%) & electric acceleration (+15 km/h)

manual.brake();      // Friction brakes
ev.brake();          // Regenerative braking charging battery
```

---

## ⚠️ Common Mistakes to Avoid

1. **Tight Coupling (God Parent)**: Parent class mein sab kuch daal dena jo har child ke liye valid na ho.
2. **Favor Composition Over Inheritance**: Har jagah inheritance mat lagao. Agar relationship sirf code sharing ke liye chahiye aur true "IS-A" nahi hai, toh Composition behtar hota hai.

---

## 📝 Lecture 03 Summary (Quick Revision)

- **Inheritance (`extends`)**: Common attributes aur behaviors ko base class mein rakho taaki code DRY (Don't Repeat Yourself) rahe.
- **IS-A vs HAS-A**: Inheritance sirf tab use karo jab Child class Parent class ka genuine type ho.
- **Overloading**: Compile-time polymorphism (same function name, different parameter types/counts).
- **Overriding**: Run-time polymorphism (subclass overrides parent method; base reference invokes child implementation dynamically).
