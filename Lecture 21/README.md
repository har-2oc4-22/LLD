# Lecture 21: Proxy Design Pattern (Virtual, Protection, Remote)

---

## 🎯 What is Proxy Pattern?

> **Definition**: *"Provide a surrogate or placeholder for another object to control access to it."*

Proxy pattern mein ek intermediary (bichauliya) object create kiya jaata hai jo original (**Real Subject**) object ke aage khada hokar request ko intercept karta hai. Proxy decide karta hai ki request real object tak kab, kaise aur kis permission ke sath jayegi.

---

## 💡 Real-World Analogy: Debit Card 💳 & CEO Secretary 🧑‍💼

1. **Debit Card**: Aapki jeb mein Debit Card ek **Proxy** hai aapke actual Bank Account Balance ka. Jab aap swipe karte ho, card check karta hai ki pin valid hai ya nahi aur bank account se deduct karwa deta hai.
2. **Company CEO's Secretary**: Agar har employee direct CEO ke cabin mein ghusne lage, toh chaos ho jayega. Secretary (Protection Proxy) check karti hai: *"Kya meeting scheduled hai? Kya permission hai?"* Tabhi andar bhejti hai.

---

## 🎭 The 3 Types of Proxy (Explained with Code)

### 1. 🖼️ Virtual Proxy (Lazy Loading / Resource Optimization)
- **Problem**: 50MB ki high-resolution images disk se pehle hi load kar loge toh app freeze ho jayegi.
- **Solution**: Virtual Proxy image ka placeholder rakhta hai. Jab tak actual `display()` call na ho, tab tak real image load nahi hoti (Lazy initialization).

```java
class ImageProxy implements IImage {
    private RealImage realImage; // null initially
    private String filename;

    public ImageProxy(String file) { this.filename = file; }

    public void display() {
        if (realImage == null) {
            // Expensive loading disk/network se sirf tab hogi jab sach me display hona ho!
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}
```

---

### 2. 🔐 Protection Proxy (Access Control / Authorization)
- **Problem**: Har user ko confidential documents read karne ka access nahi hona chahiye.
- **Solution**: Proxy check karta hai ki user ke paas required role (e.g., `premiumMembership == true`) hai ya nahi.

```java
class DocumentProxy implements IDocumentReader {
    private RealDocumentReader realReader = new RealDocumentReader();
    private User user;

    public DocumentProxy(User user) { this.user = user; }

    public void unlockPDF(String path, String password) {
        if (!user.premiumMembership) {
            System.out.println("Access Denied: Premium account required!");
            return; // Blocked at proxy level!
        }
        realReader.unlockPDF(path, password);
    }
}
```

---

### 3. 🌐 Remote Proxy (Network / RPC Communication)
- **Problem**: Real object kisi doosre server / microservice par rehta hai.
- **Solution**: Remote proxy local machine par rehta hai aur internal socket / HTTP serialization handle karke remote machine se result laa deta hai (e.g., gRPC stubs, RMI, Java Spring `@FeignClient`).

---

## ⚖️ Proxy vs Decorator vs Adapter

| Pattern | Primary Intent | Does it modify Interface? |
| :--- | :--- | :--- |
| **Proxy** | Control **access / lifecycle** to real object | No, implements exact same interface |
| **Decorator** | Add **new behaviors / responsibilities** | No, implements same interface + adds behavior |
| **Adapter** | Fix **incompatible interfaces** | Yes, provides target interface client expects |

---

## 📝 Lecture 21 Summary (Quick Revision)

- **Interception Layer**: Real object tak pahunchne se pehle caching, security check, lazy loading ya logging add karta hai.
- **Same Interface**: Proxy aur Real Subject dono same interface implement karte hain, isliye client ko pata bhi nahi chalta ki wo proxy se baat kar raha hai ya real object se.
- **Three Flavors**: Virtual (Lazy loading), Protection (Security & Auth), Remote (Network RPC).
