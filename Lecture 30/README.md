# Lecture 30: Flyweight Design Pattern

---

## 🎯 What is Flyweight Pattern?

> **Definition**: *"Use sharing to support large numbers of fine-grained objects efficiently."*

Flyweight pattern ka primary purpose hota hai **RAM (Memory) bachaana** jab system ko hazaro ya lakho chote-chote objects render ya maintain karne hote hain.

---

## 💡 Real-World Analogy: 1,000,000 Asteroids in a Space Game 🚀 🪨

Maan lo aap ek Space Shooter Game bana rahe ho jisme screen par **10 Lakh (1,000,000) Asteroids** fly karte hain:
- Har asteroid ke paas 3D Mesh, 4K Texture image, Material properties, aur Color hota hai (heavy data ~ 128 bytes).
- Har asteroid ke paas apni screen coordinates `(posX, posY)` aur speed `(velocityX, velocityY)` hoti hai (lightweight data ~ 16 bytes).

### ❌ Without Flyweight:
- Agar aap 10 Lakh individual objects banaoge, jisme har object 128 bytes texture data duplicate store karega:
- **Total RAM = $1,000,000 \times 128\text{ bytes} \approx 128\text{ MB to } 500\text{ MB}$**! Game crash ho jayegi (OutOfMemoryError).

### ✅ With Flyweight:
- Pure game mein sirf **3 types ke Asteroids** hain: Rocky, Metallic, Icy.
- Heavy textures sirf **3 Flyweight Objects** mein memory mein rahenge.
- 10 Lakh objects sirf un 3 flyweights ka pointer aur apna `(x, y)` coordinate store karenge.
- **RAM usage 80% se zyada drop ho jaata hai!**

---

## 🧠 The Core Concept: Intrinsic vs Extrinsic State

| State Type | Characteristics | Where it is Stored | Example |
| :--- | :--- | :--- | :--- |
| **Intrinsic State** | **Shared, constant, immutable**. Kisi asteroid ke coordinate badalne se uska texture nahi badalta. | **Inside Flyweight object** | 3D Texture, Color, Weight |
| **Extrinsic State** | **Unique, context-dependent**. Har asteroid ka position aur velocity alag hota hai. | **Inside Context object** | `(x, y)` position, speed |

---

## 🏗️ Architecture

```text
                     [AsteroidFactory] (Cache/Pool)
                    Map<String, AsteroidFlyweight>
                            │ returns shared flyweight
                            ▼
           [AsteroidFlyweight] (Intrinsic State: 3D Mesh, Texture)
                            ▲
                            │ references
           [AsteroidContext] (Extrinsic State: posX, posY, velocity)
```

---

## 💻 Code Walkthrough

```java
// 1. Flyweight: Stores ONLY shared intrinsic data
class AsteroidFlyweight {
    private String color;
    private String texture; // Heavy asset!
    private String material;

    public AsteroidFlyweight(String c, String t, String m) {
        this.color = c; this.texture = t; this.material = m;
    }

    // Extrinsic state is passed as arguments into render()
    public void render(int x, int y, int velX, int velY) {
        System.out.println("Rendering " + color + " at (" + x + "," + y + ")");
    }
}

// 2. Flyweight Factory (Object Pool)
class AsteroidFactory {
    private static Map<String, AsteroidFlyweight> pool = new HashMap<>();

    public static AsteroidFlyweight getAsteroid(String color, String texture, String material) {
        String key = color + "_" + texture + "_" + material;
        if (!pool.containsKey(key)) {
            pool.put(key, new AsteroidFlyweight(color, texture, material));
        }
        return pool.get(key); // Returns cached shared instance!
    }
}

// 3. Context: Holds extrinsic data + pointer to shared flyweight
class AsteroidContext {
    private AsteroidFlyweight flyweight; // Just an 8-byte reference!
    private int posX, posY;

    public AsteroidContext(AsteroidFlyweight fw, int x, int y) {
        this.flyweight = fw;
        this.posX = x; this.posY = y;
    }

    public void render() {
        flyweight.render(posX, posY, 1, 2);
    }
}
```

---

## 📝 Lecture 30 Summary (Quick Revision)

- **Memory Optimization**: Massive number of similar objects ke memory footprint ko radically shrink karta hai.
- **Separate States**: Intrinsic (shared) ko Extrinsic (unique context) se alag karo.
- **Flyweight Factory**: Pool/Cache maintain karta hai taaki already created intrinsic flyweights reuse ho sakein.
- **Real-World Examples**: Java `String.intern()`, `Integer.valueOf(127)` cache (-128 to 127), Word Processors (char glyph rendering).
