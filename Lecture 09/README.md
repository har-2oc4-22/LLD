# Lecture 09: Factory Patterns (Simple Factory, Factory Method, Abstract Factory)

---

## 🎯 What are Factory Patterns?

Creational Design Patterns ka main goal hota hai **Object Creation Logic** ko client code se isolate karna.
Agar client code har jagah `new ConcreteClass()` likhega, toh code tightly coupled ho jayega. Factory patterns object instantiations ko manage karte hain.

Lecture 09 mein 3 patterns seekhe jaate hain:
1. **Simple Factory** (Not a GoF pattern, but widely used idiom)
2. **Factory Method** (GoF Creational Pattern)
3. **Abstract Factory** (GoF Creational Pattern - Factory of Factories)

---

## 1. 🏭 Simple Factory

### 💡 Real-World Analogy
Aap kisi Coffee Shop ke counter par jaate ho aur bolte ho *"Bhaiya, ek Cappuccino bana do"*. Aap khud kitchen mein jaakar milk steam nahi karte, coffee beans grind nahi karte. Counter ka barista ek **Factory** hai jo aapko ready-made object de deta hai.

```java
class SimpleBurgerFactory {
    public static Burger createBurger(String type) {
        if (type.equalsIgnoreCase("basic")) return new BasicBurger();
        if (type.equalsIgnoreCase("cheese")) return new CheeseBurger();
        throw new IllegalArgumentException("Unknown type");
    }
}
```
- **Limitation**: Naya burger aane par factory class ke `if-else` ko modify karna padta hai (OCP violation).

---

## 2. 🍔 Factory Method Pattern

> **Definition**: *"Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses."*

### 💡 Real-World Analogy
Socho do burger franchise hain:
1. **SinghBurger**: Regular Maida buns use karta hai.
2. **KingBurger**: Healthy Whole-Wheat (atta) buns use karta hai.

Client ko basic burger chahiye, lekin franchise brand decide karega ki burger exactly kaisa banega!

### 💻 Code Architecture
```text
           [BurgerFactory] (Interface)
             createBurger()
              /          \
    [SinghBurger]      [KingBurger]
     (creates            (creates
   Normal Burgers)     Wheat Burgers)
```

```java
interface BurgerFactory {
    Burger createBurger(String type);
}

class SinghBurger implements BurgerFactory {
    public Burger createBurger(String type) {
        if (type.equalsIgnoreCase("basic")) return new BasicBurger();
        return new PremiumBurger();
    }
}

class KingBurger implements BurgerFactory {
    public Burger createBurger(String type) {
        if (type.equalsIgnoreCase("basic")) return new BasicWheatBurger();
        return new PremiumWheatBurger();
    }
}
```

---

## 3. 🍱 Abstract Factory Pattern (Factory of Factories)

> **Definition**: *"Provide an interface for creating families of related or dependent objects without specifying their concrete classes."*

### 💡 Real-World Analogy: UI Themes / Meal Combos
Agar aap **Dark Theme** select karte ho, toh Button bhi Dark hona chahiye aur Scrollbar bhi Dark hona chahiye. Dark Button ke sath Light Scrollbar mix nahi hona chahiye!
Waise hi, agar aap **KingBurger (Wheat franchise)** se Meal order kar rahe ho, toh **Burger bhi Wheat** hona chahiye aur sath mein **Garlic Bread bhi Wheat** honi chahiye!

```java
// Abstract Factory creates a FAMILY of related products
interface MealFactory {
    Burger createBurger(String type);
    GarlicBread createGarlicBread(String type);
}

class SinghBurger implements MealFactory {
    public Burger createBurger(String type) { return new BasicBurger(); }
    public GarlicBread createGarlicBread(String type) { return new BasicGarlicBread(); }
}

class KingBurger implements MealFactory {
    public Burger createBurger(String type) { return new BasicWheatBurger(); }
    public GarlicBread createGarlicBread(String type) { return new BasicWheatGarlicBread(); }
}
```

---

## ⚖️ Quick Comparison Table

| Pattern | What It Creates | How It Scales | Complexity |
| :--- | :--- | :--- | :--- |
| **Simple Factory** | Single object based on parameter | Modification of single factory class | Low |
| **Factory Method** | Single object via inheritance/subclass | Add a new Factory subclass | Medium |
| **Abstract Factory** | Entire **family** of related products | Add a new Factory producing all family items | High |

---

## 📝 Lecture 09 Summary (Quick Revision)

- **Eliminate `new` in client**: Object creation ko separate layer par delegate karo.
- **Factory Method**: Ek product hierarchy ke liye subclasses ko creation decide karne do.
- **Abstract Factory**: Jab multiple related products ko ek sath consistent rakhna ho (e.g. MacButton + MacCheckbox vs WinButton + WinCheckbox).
