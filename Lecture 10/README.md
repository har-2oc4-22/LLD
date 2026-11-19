# Lecture 10: Singleton Design Pattern

---

## 🎯 What is Singleton Pattern?

> **Definition**: *"Ensure a class has only ONE instance, and provide a global point of access to it."*

---

## 💡 Real-World Analogy: The President of a Country

Ek desh mein lakho log hote hain, lekin **President ya Prime Minister** sirf ek hi hota hai.
Chahe koi bhi citizen unhe access kare ya unki baat sune, sabhi same single person ko refer kar rahe hote hain.

Software mein:
- **Database Connection Pool**: Har query ke liye naya pool object nahi banate; poora app ek hi pool use karta hai.
- **Logger**: Poori application ka logs ek single centralized file logger par likha jaata hai.
- **Configuration Manager**: `application.properties` ya `config.json` ka data ek hi baar memory mein load hota hai.

---

## 🏗️ The 3 Golden Rules of Singleton

1. **Private Constructor**: Bahar se koi bhi `new Singleton()` call na kar sake.
2. **Private Static Instance**: Class apne single instance ko internally hold karti hai.
3. **Public Static Getter (`getInstance()`)**: Global access point jo wahi single instance return karta hai.

---

## 🧗 The Evolution: From Naive to Production-Ready

### 1. ❌ Naive Lazy Initialization (Single-Threaded Only)
```java
public class SimpleSingleton {
    private static SimpleSingleton instance = null;
    private SimpleSingleton() {}

    public static SimpleSingleton getInstance() {
        if (instance == null) {
            instance = new SimpleSingleton(); // MULTITHREADING TRAP!
        }
        return instance;
    }
}
```
*Issue*: Agar do threads simultaneously `if (instance == null)` evaluate karein, toh dono threads constructor call karke 2 alag-alag objects bana denge!

---

### 2. ⚡ Eager Initialization (Thread-Safe, Resource Wasteful)
```java
public class EagerSingleton {
    // Class load hote hi object ban gaya
    private static final EagerSingleton instance = new EagerSingleton();
    private EagerSingleton() {}

    public static EagerSingleton getInstance() {
        return instance;
    }
}
```
*Trade-off*: Thread-safe hai, lekin agar object bahut heavy hai (e.g. 500MB DB cache) aur application ne kabhi use hi nahi kiya, toh bina wajah memory block ho jayegi.

---

### 3. 🐢 Synchronized Method (Thread-Safe, Performance Bottleneck)
```java
public static synchronized ThreadSafeLockingSingleton getInstance() {
    if (instance == null) {
        instance = new ThreadSafeLockingSingleton();
    }
    return instance;
}
```
*Trade-off*: Thread-safe hai, lekin har bar `getInstance()` call hone par lock acquire hoga. 99.9% calls sirf already-created instance read karne ke liye hoti hain, toh unnecessary lock se app slow ho jaata hai.

---

### 4. 🚀 Double-Checked Locking (Industry Standard)
Lock sirf pehli baar create karte waqt lagao, read karte waqt nahi!

```java
public class ThreadSafeDoubleLockingSingleton {
    // volatile ensures instruction reordering doesn't cause partially initialized object leakage
    private static volatile ThreadSafeDoubleLockingSingleton instance = null;

    private ThreadSafeDoubleLockingSingleton() {}

    public static ThreadSafeDoubleLockingSingleton getInstance() {
        // First check: Lock acquire karne se pehle (Fast read path)
        if (instance == null) {
            synchronized (ThreadSafeDoubleLockingSingleton.class) {
                // Second check: Lock milne ke baad (Thread-safety guarantee)
                if (instance == null) {
                    instance = new ThreadSafeDoubleLockingSingleton();
                }
            }
        }
        return instance;
    }
}
```

---

## ⚖️ Singleton Approaches Comparison

| Approach | Thread Safe? | Lazy Loaded? | Performance |
| :--- | :--- | :--- | :--- |
| **Simple Lazy** | ❌ No | ✅ Yes | Fast (Buggy in multi-thread) |
| **Eager** | ✅ Yes | ❌ No | Fast (High memory usage) |
| **Synchronized Method** | ✅ Yes | ✅ Yes | 🐢 Slow (Lock contention) |
| **Double-Checked Locking** | ✅ Yes | ✅ Yes | ⚡ Optimal (Best of all worlds) |

---

## 📝 Lecture 10 Summary (Quick Revision)

- **One Instance Only**: Singleton ensures system resource (DB, Logger) duplicate na ho.
- **Private Constructor**: Essential to block direct instantiation.
- **Double-Checked Locking**: First check skips synchronized block if instance already exists; second check prevents race condition during initialization.
- **Volatile Keyword**: Prevents JVM instruction reordering in multithreading.
