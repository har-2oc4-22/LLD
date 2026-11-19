# Lecture 17: Facade Design Pattern

---

## 🎯 What is Facade Pattern?

> **Definition**: *"Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use."*

Jab kisi system ke andar 10 alag-alag complex classes aur subsystems hote hain, toh client ko un sabhi ke internal technical steps manage karne se bachane ke liye hum saamne ek simple **"Facade" (Front Desk / Front Window)** khada kar dete hain.

---

## 💡 Real-World Analogy: PC Power Button 💻 & Hotel Receptionist 🛎️

### 1. Computer Booting
Jab aap subah laptop ka **Power Button** press karte ho:
- Behind the scenes kya hota hai?
  1. `PowerSupply.providePower()`
  2. `CoolingSystem.startFans()`
  3. `BIOS.boot()`
  4. `CPU.initialize()`
  5. `Memory.selfTest()`
  6. `HardDrive.spinUp()`
  7. `OperatingSystem.load()`
- Socho agar user ko har subah terminal me ye 7 commands manually type karni padti!
- **ComputerFacade** ne user ko sirf ek method diya: `startComputer()`.

### 2. Hotel Reception
Aap hotel me check-in karne jaate ho. Aap khud laundry room, kitchen chef, bellboy aur room cleaner ke paas alag-alag nahi jaate. Aap sirf **Receptionist (Facade)** se baat karte ho, wo internal subsystems ko internally coordinate kar leti hai.

---

## 🏗️ Architecture: Subsystems vs Facade

```text
                  [Client]
                     │
                     ▼
             [ComputerFacade] (Single Entry Point)
              startComputer()
         ┌───────────┼───────────┬───────────┐
         ▼           ▼           ▼           ▼
   [PowerSupply]   [Fans]      [BIOS]       [OS]
  (Subsystem 1) (Subsystem 2) (Subsystem 3) (Subsystem 4)
```

---

## 💻 Code Walkthrough

```java
// Complex Subsystems
class PowerSupply { void providePower() { ... } }
class CoolingSystem { void startFans() { ... } }
class BIOS { void boot(CPU cpu, Memory mem) { ... } }
class OperatingSystem { void load() { ... } }

// Facade coordinates everything in sequence
class ComputerFacade {
    private PowerSupply power = new PowerSupply();
    private CoolingSystem fans = new CoolingSystem();
    private BIOS bios = new BIOS();
    private CPU cpu = new CPU();
    private Memory mem = new Memory();
    private OperatingSystem os = new OperatingSystem();

    public void startComputer() {
        power.providePower();
        fans.startFans();
        bios.boot(cpu, mem);
        os.load();
        System.out.println("Computer Booted Successfully!");
    }
}

// Client has a clean, one-line interface
public class FacadePattern {
    public static void main(String[] args) {
        ComputerFacade pc = new ComputerFacade();
        pc.startComputer(); // Clean and simple!
    }
}
```

---

## ⚖️ Facade vs Adapter (Key Differences)

| Parameter | Facade Pattern | Adapter Pattern |
| :--- | :--- | :--- |
| **Purpose** | Subsystem ko **easy & simple** banana | Do incompatible interfaces ko **compatible** banana |
| **Number of Classes** | Usually wraps multiple complex subsystem classes | Usually wraps a single incompatible class |
| **Interface** | Naya simplified high-level interface introduce karta hai | Existing target interface ko match karta hai |

---

## 📝 Lecture 17 Summary (Quick Revision)

- **Simplicity First**: Complex internal workflows ko hide karke clean high-level API provide karta hai.
- **Loose Coupling**: Client direct subsystem classes se couple nahi hota; agar internal subsystem me koi class change ho toh client break nahi hota.
- **Subsystems remain accessible**: Agar kisi advanced user ko directly `CPU.initialize()` call karna ho, toh subsystem abhi bhi directly available rehta hai (Facade usage optional hota hai).
