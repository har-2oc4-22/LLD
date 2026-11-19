# Lecture 16: Adapter Design Pattern

---

## 🎯 What is Adapter Pattern?

> **Definition**: *"Convert the interface of a class into another interface that clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces."*

Adapter pattern do incompatible interfaces ke beech **Translator / Bridge** ka kaam karta hai bina dono ke existing source code ko modify kiye.

---

## 💡 Real-World Analogy: Travel Adapter & USB-C Dongle 🔌

1. **Travel Adapter**:
   - Aap US se ek laptop purchase karke India laaye. Uska charger **Flat 2-Pin (US Plug)** hai.
   - India ke wall sockets **Round 3-Pin (Indian Socket)** support karte hain.
   - Na toh aap wall socket tod sakte ho, aur na hi laptop charger ka wire kaat sakte ho.
   - Aap bazaar se ek **Travel Adapter** khareedte ho: Wo ek taraf se US pin accept karta hai aur doosri taraf se Indian socket me plug hota hai!

2. **Legacy Data Adapter**:
   - Aapka modern React / Mobile frontend sirf **JSON** format samajhta hai.
   - Purana Legacy Backend Service sirf **XML** format return karta hai.
   - Hum ek **XML-to-JSON Adapter** laga dete hain jo XML ko silently JSON mein transform kar deta hai.

---

## 🏗️ Architecture: Target, Adaptee & Adapter

```text
[Client] ── calls ──> [Target Interface] (IReports: getJsonData())
                             ▲
                             │ implements
                     [Adapter Class] (XmlDataProviderAdapter)
                             │ wraps (HAS-A)
                             ▼
                     [Adaptee Class] (XmlDataProvider: getXmlData())
```

---

## 💻 Code Walkthrough

```java
// 1. Target Interface: What modern client expects
interface IReports {
    String getJsonData(String data);
}

// 2. Adaptee: Existing incompatible legacy class
class XmlDataProvider {
    String getXmlData(String data) {
        return "<user><name>Alice</name><id>42</id></user>";
    }
}

// 3. Adapter: Implements Target + Wraps Adaptee
class XmlDataProviderAdapter implements IReports {
    private XmlDataProvider xmlProvider;

    public XmlDataProviderAdapter(XmlDataProvider provider) {
        this.xmlProvider = provider;
    }

    @Override
    public String getJsonData(String data) {
        // Step 1: Call adaptee
        String xml = xmlProvider.getXmlData(data);

        // Step 2: Convert XML to JSON format
        String name = extractTag(xml, "name");
        String id   = extractTag(xml, "id");

        // Step 3: Return translated target format
        return "{\"name\":\"" + name + "\", \"id\":" + id + "}";
    }
}
```

---

## ⚖️ Class Adapter vs Object Adapter

- **Object Adapter (Recommended & Used in code)**: Composition use karta hai (`Adapter HAS-A Adaptee`). Flexible hota hai aur Kisi bhi subclass of Adaptee ke sath kaam kar sakta hai.
- **Class Adapter**: Multiple Inheritance use karta hai (`Adapter extends Adaptee, implements Target`). Java mein multiple inheritance allowed nahi hoti, isliye Object Adapter universally preferred hai.

---

## 📝 Lecture 16 Summary (Quick Revision)

- **Compatibility Bridge**: Existing working classes ko bina chhue naye system ke sath compatible banata hai.
- **Single Responsibility Principle**: Data conversion aur translation logic core business logic se bahar isolated rehti hai.
- **Open/Closed Principle**: Naye adapters bina existing clients ya adaptees ko disturb kiye add kiye ja sakte hain.
