# Lecture 36: Prototype Design Pattern

---

## 🎯 What is Prototype Pattern?

> **Definition**: *"Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype."*

Jab kisi object ko `new` keyword se instantiate karna bahut **expensive** hota hai (e.g. database query, network call, heavy 3D assets loading, complex calculations), tab Prototype pattern already existing object ka ek **Clone (Exact Copy)** bana deta hai aur hum us copy ko tweak/modify kar lete hain.

---

## 💡 Real-World Analogy: Xerox Machine & Game NPCs 👾

1. **Photocopy (Xerox)**:
   - Agar aapko 10 page ka lamba application form 5 logon ko dena hai, toh aap 5 bar haath se poora form nahi likhte.
   - Aap ek master copy (Prototype) banate ho aur Xerox machine se 5 copies print karke unme sirf Candidate Name alag se fill kar lete ho.
2. **Video Game Enemies (NPCs)**:
   - Ek game mein 500 Alien enemies spawn karni hain. Har alien ka 3D skeleton aur weapon load karne mein 200ms lagte hain.
   - 500 bar DB se load karenge toh game lag karegi!
   - Hum 1 master alien create karte hain, aur baaki 499 aliens ko milliseconds mein **`.clone()`** kar lete hain!

---

## 🔬 Shallow Copy vs Deep Copy (Interview Essential!)

- **Shallow Copy**: Sirf primitive fields copy hoti hain. Agar object ke andar koi doosra nested object reference hai (e.g. `Address` ya `Weapon`), toh clone aur original dono **same memory address** ko point karte hain. Ek me change karne se doosra affect ho sakta hai.
- **Deep Copy**: Original object ke sath-sath uske andar ke sabhi nested objects ki bhi nayi copies create hoti hain in independent memory locations.

---

## 💻 Code Walkthrough

```java
// 1. Prototype Interface
interface Cloneable {
    Cloneable clone();
}

// 2. Concrete Prototype
class NPC implements Cloneable {
    public String name;
    public int health;
    public int attack;

    // Heavy constructor (expensive initialization)
    public NPC(String name, int health, int attack) {
        // Expensive DB / Network / Physics calculations...
        this.name = name;
        this.health = health;
        this.attack = attack;
    }

    // Copy Constructor used for cloning
    public NPC(NPC other) {
        this.name = other.name;
        this.health = other.health;
        this.attack = other.attack;
    }

    @Override
    public Cloneable clone() {
        return new NPC(this); // Fast in-memory clone!
    }
}

// 3. Client Usage
public class PrototypePattern {
    public static void main(String[] args) {
        // Build one heavy master template
        NPC baseAlien = new NPC("Alien Grunt", 100, 15);

        // Instantly clone and tweak attributes
        NPC bossAlien = (NPC) baseAlien.clone();
        bossAlien.name = "Alien Boss";
        bossAlien.health = 500;
    }
}
```

---

## 📝 Lecture 36 Summary (Quick Revision)

- **Creation by Cloning**: Bypasses heavy constructors and initialization logic.
- **Copy Constructor / Clone Method**: Object khud ko clone karne ka logic encapsulate karta hai.
- **Tweak on the Fly**: Cloned object ke specific properties ko independent modify kiya ja sakta hai.
- **Java Built-in Support**: Java ka `java.lang.Cloneable` marker interface aur `Object.clone()` method isi pattern par based hai.
