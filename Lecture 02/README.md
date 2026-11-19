# Lecture 02: Abstraction & Encapsulation

---

## 🎯 Concepts at a Glance

OOP ke pehle do pillars:
1. **Abstraction**: *"WHAT an object does, not HOW it does it"* (Complexity chupana aur simple interface expose karna).
2. **Encapsulation**: *"Data + Functions ko wrap karna aur internal state ko protect karna"* (Data security & validation).

---

## 1. 🧩 Abstraction Explained

### 💡 Real-World Analogy
Jab aap car drive karte ho:
- Aapke paas **Steering Wheel, Accelerator Pedal, aur Brake Pedal** hote hain.
- Jab aap accelerator press karte ho, aapko andar ke spark plugs, fuel injection nozzles, ya piston movements ki chinta nahi karni padti.
- Car ne aapko ek simple interface diya: `accelerate()`. Andar ka complex mechanism **abstracted (hidden)** hai.

### 💻 Code Implementation
Abstraction ko achieve karne ke liye hum **Interfaces** ya **Abstract Classes** ka use karte hain:

```java
// What the car can do (Contract)
interface Car {
    void startEngine();
    void accelerate();
    void brake();
    void stopEngine();
}

// How it actually does it (Concrete Implementation)
class SportsCar implements Car {
    private int currentSpeed = 0;

    @Override
    public void accelerate() {
        currentSpeed += 20;
        System.out.println("Accelerating to: " + currentSpeed + " km/h");
    }
    // ... other methods implemented
}
```

---

## 2. 🛡️ Encapsulation Explained

### 💡 Real-World Analogy: Medical Capsule
Medical capsule mein alag-alag medicines ko ek single protective shell ke andar pack kiya jaata hai taaki bahar ki gandagi se bachi rahe aur body mein controlled way mein release ho.

Programming mein:
- Variables aur Methods ko class ke andar wrap karna.
- Variables ko `private` banana taaki koi bahar se invalid state set na kar sake.
- **Invariants protect karna**: Jaise agar `speed` direct public hoti, toh koi `car.speed = -500;` kar deta jo physical world mein impossible hai!

### 💻 Code Implementation

```java
class SportsCar {
    // 1. Data Hiding (Private variables)
    private int currentSpeed = 0;
    private boolean isEngineOn = false;

    // 2. Controlled Access via Methods
    public void accelerate() {
        if (!isEngineOn) {
            System.out.println("Engine is off! Cannot accelerate.");
            return; // Invariant check: engine band hone par accelerate nahi ho sakta
        }
        currentSpeed += 20;
    }

    public int getSpeed() {
        return currentSpeed; // Read-only access
    }
}
```

---

## ⚖️ Abstraction vs Encapsulation (Key Differences)

| Feature | Abstraction | Encapsulation |
| :--- | :--- | :--- |
| **Focus** | Complexity ko hide karna (*Outer View*) | Data ko protect karna (*Inner View*) |
| **Question** | "WHAT does this object do?" | "HOW is the data safely kept?" |
| **How to achieve** | Interfaces aur Abstract Classes ke through | Access Modifiers (`private`, `protected`) aur Methods ke through |
| **Example** | TV Remote ke buttons | Remote ke andar ki circuit board and battery packaging |

---

## 📝 Lecture 02 Summary (Quick Revision)

- **Abstraction**: Bahar ke user ko sirf zaroori controls expose karo, implementation details hide karo.
- **Encapsulation**: Data aur behavior ko bind karo, variables ko private rakho taaki koi direct manipulate na kare aur business rules (invariants) hamesha valid rahein.
- **Best Practice**: Hamesha variables ko private rakho aur sirf zaroorat padne par unke controlled methods ya getters provide karo.
