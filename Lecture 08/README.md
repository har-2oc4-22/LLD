# Lecture 08: Strategy Design Pattern

---

## 🎯 What is Strategy Pattern?

> **Definition**: *"Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it."*

---

## 💡 Real-World Analogy: Google Maps Navigation

Jab aap Google Maps mein Destination daalte ho:
- Agar aap **Car** se ja rahe ho: Route calculation highways aur fast speed roads prefer karega.
- Agar aap **Walking** select karte ho: Route pedestrian paths, stairs aur shortcuts dikhayega.
- Agar aap **Public Transport** select karte ho: Bus aur Metro schedules use karega.

Yahan `RouteStrategy` ek family of algorithms hai jo runtime par dynamically interchange hoti hai bina Google Maps ke core app ko change kiye!

---

## ❓ The Problem with Pure Inheritance (Why We Need Strategy?)

Maan lo aapke paas alag-alag robots hain (`CompanionRobot`, `WorkerRobot`, `DroneRobot`).
- Kuch robots normal walk kar sakte hain, kuch nahi chal sakte.
- Kuch robots fly kar sakte hain, kuch nahi ud sakte.
- Agar aap ye sab base class `Robot` mein daal doge, toh jo robot fly nahi kar sakta usme bhi `fly()` method inherit ho jayega!
- Agar aap subclasses mein duplicate code likhoge, toh OCP aur DRY break ho jayega.

**Solution: "Favor Composition over Inheritance"**
Robots ko behaviors inherit karne ki bajaye, behaviors ko **inject (compose)** karo!

---

## 🏗️ Architecture & Structure

```text
                  [Robot] (Context)
                     │ has-a
        ┌────────────┼────────────┐
        ▼            ▼            ▼
 [WalkableRobot] [TalkableRobot] [FlyableRobot] (Strategy Interfaces)
   ▲        ▲      ▲        ▲      ▲       ▲
   │        │      │        │      │       │
[Normal]  [No]  [Normal]  [No]  [Normal]  [No]  (Concrete Strategies)
```

---

## 💻 Code Walkthrough

### 1. Strategy Interfaces & Implementations
```java
// Strategy Interface
interface FlyableRobot {
    void fly();
}

// Concrete Strategies
class NormalFly implements FlyableRobot {
    public void fly() { System.out.println("Flying normally..."); }
}

class NoFly implements FlyableRobot {
    public void fly() { System.out.println("Cannot fly."); }
}
```

### 2. Context Class (Robot)
```java
abstract class Robot {
    protected WalkableRobot walkBehavior;
    protected TalkableRobot talkBehavior;
    protected FlyableRobot flyBehavior;

    public Robot(WalkableRobot w, TalkableRobot t, FlyableRobot f) {
        this.walkBehavior = w;
        this.talkBehavior = t;
        this.flyBehavior = f;
    }

    public void fly() {
        flyBehavior.fly(); // Delegated to strategy!
    }
}
```

### 3. Client Usage
```java
// Companion robot: Walks, talks, but does NOT fly
Robot companion = new CompanionRobot(new NormalWalk(), new NormalTalk(), new NoFly());
companion.fly(); // Output: Cannot fly.

// Worker robot: Flies, cannot talk
Robot drone = new WorkerRobot(new NoWalk(), new NoTalk(), new NormalFly());
drone.fly(); // Output: Flying normally...
```

---

## 📝 Lecture 08 Summary (Quick Revision)

- **Core Idea**: Behaviors ko separate classes (strategies) mein encapsulate karo aur context class mein inject karo.
- **Composition over Inheritance**: Inheritance ki jagah HAS-A relationship use karo.
- **Runtime Flexibility**: Runtime par strategy change ki ja sakti hai (e.g. `robot.setFlyBehavior(new JetpackFly())`).
- **When to Use**: Jab ek hi kaam karne ke multiple tareeqe (algorithms/behaviors) hon aur aap unhe bina if-else chains ke dynamically switch karna chahte hon (e.g., Payment Strategies like UPI, Card, NetBanking).
