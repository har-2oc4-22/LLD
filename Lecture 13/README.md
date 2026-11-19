# Lecture 13: Decorator Design Pattern

---

## 🎯 What is Decorator Pattern?

> **Definition**: *"Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality."*

Decorator pattern allows you to **wrap** an existing object inside another object to add new features without changing the original class code.

---

## 💡 Real-World Analogy: Pizza Toppings & Mario Power-Ups 🍄

### 1. Pizza Customization
- Aapne order kiya: **Base Pizza (Rs 150)**.
- Extra Topping: **Cheese Burst (+ Rs 50)**.
- Extra Topping: **Olives (+ Rs 30)**.
- Final Cost = 150 + 50 + 30 = 230.
Har topping base pizza ko wrap karke uski price aur description decorate kar rahi hai.

### 2. Mario Game
- Base **Mario** normal run karta hai.
- Mushroom milte hi: `HeightUp(Mario)`.
- Flower milte hi: `GunPowerUp(HeightUp(Mario))`.
- Star milte hi: `StarPowerUp(GunPowerUp(HeightUp(Mario)))`.

---

## 💣 The "Class Explosion" Problem (Why Subclassing Fails)

Agar hum power-ups ke liye Inheritance use karein:
- `Mario`
- `HeightMario`
- `GunMario`
- `StarMario`
- `HeightAndGunMario`
- `HeightAndStarMario`
- `HeightAndGunAndStarMario`
Agar 5 power-ups huye toh **$2^5 = 32$ alag-alag classes** banani padengi! Isse kehte hain **Class Explosion**.

**Solution**: Object composition via Decorator pattern!

---

## 🏗️ Architecture & Anatomy

The most fascinating part of Decorator pattern is:
> **Decorator IS-A Component AND HAS-A Component!**

```text
                  [Character] (Component Interface)
                     getAbilities()
                     /           \
                    /             \
            [Mario]                [CharacterDecorator] (Abstract Decorator)
      (Concrete Component)          ├── character: Character (HAS-A)
                                    └── getAbilities() (IS-A)
                                           /        \
                             [HeightUp]         [GunPowerUp]
```

---

## 💻 Code Walkthrough

```java
// 1. Component Interface
interface Character {
    String getAbilities();
}

// 2. Concrete Base Object
class Mario implements Character {
    public String getAbilities() { return "Mario"; }
}

// 3. Decorator Base (Both implements & holds reference)
abstract class CharacterDecorator implements Character {
    protected Character character; // Wrapped object
    public CharacterDecorator(Character c) { this.character = c; }
}

// 4. Concrete Decorators
class HeightUp extends CharacterDecorator {
    public HeightUp(Character c) { super(c); }
    public String getAbilities() {
        return character.getAbilities() + " + HeightUp";
    }
}

class GunPowerUp extends CharacterDecorator {
    public GunPowerUp(Character c) { super(c); }
    public String getAbilities() {
        return character.getAbilities() + " + Gun";
    }
}
```

### 5. Client Usage: Dynamic Wrapping (Russian Matryoshka Dolls)
```java
Character mario = new Mario();
mario = new HeightUp(mario);      // Wrapped in HeightUp
mario = new GunPowerUp(mario);    // Wrapped in GunPowerUp

System.out.println(mario.getAbilities()); 
// Output: Mario + HeightUp + Gun
```

---

## 📝 Lecture 13 Summary (Quick Revision)

- **Dynamic Extension**: Runtime par objects ko features se decorate karo bina base class chhue.
- **Prevents Class Explosion**: $2^N$ subclasses banane ki zaroorat nahi rehti.
- **Java Real-World Example**: Java I/O Streams (`new BufferedReader(new InputStreamReader(new FileInputStream("file.txt")))`) is the classic real-world decorator!
