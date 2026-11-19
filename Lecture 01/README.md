# Lecture 01: Introduction to Object-Oriented Programming & Low-Level Design (LLD)

---

## 🎯 What is Low-Level Design (LLD)?

Jab bhi hum koi software banate hain, toh architecture ke do main levels hote hain:
1. **High-Level Design (HLD)**: System ka macro view — Databases, Caches, Load Balancers, Microservices, Network Protocols wagera.
2. **Low-Level Design (LLD)**: System ka micro view — Classes, Objects, Design Patterns, Interfaces, Methods, Data Structures aur unke beech ka interaction.

> **Simple Words mein:** HLD batata hai ki *kya-kya systems banenge*, jabki LLD batata hai ki *har service ke andar ka code kaise structured, clean aur maintainable hoga*.

---

## 💡 Real-World Analogy: Car Manufacturing

Socho aapko ek Car banani hai:
- **HLD** ye decide karega ki car ke paas Engine, 4 Tyres, Fuel Tank aur Battery hogi, aur fuel tank engine ko petrol supply karega.
- **LLD** ye decide karega ki Engine class ke andar kaunse variables (`horsepower`, `currentRPM`) honge, gear shift karne ka exact algorithm kya hoga, aur Engine fail hone par exception kaise handle hogi.

---

## 🔍 Key OOP Foundations

LLD ka base hai **Object-Oriented Programming (OOP)**. OOP ke 4 core pillars hote hain:

1. **Classes & Objects**:
   - **Class**: Ek blueprint ya template hota hai (jaise house ka architectural map).
   - **Object**: Us blueprint ka real-world physical instance (jo actual memory leta hai).
2. **State & Behavior**:
   - **State (Attributes/Variables)**: Object ke paas kya data hai (e.g., `speed`, `fuelLevel`).
   - **Behavior (Methods/Functions)**: Object kya-kya action perform kar sakta hai (e.g., `accelerate()`, `applyBrakes()`).

---

## ❓ Why Good LLD is Critical in Real Life?

1. **Maintainability**: Jab 6 mahine baad naya feature add karna ho, toh purana code break na ho.
2. **Readability & Extensibility**: Koi naya developer team join kare toh wo asani se samajh sake aur naye features easily plug-in kar sake.
3. **Testability**: Clean classes ko unit test karna bahut easy hota hai.
4. **Avoid Code Smells**: Spagetti code, God Classes (classes jo sab kuch khud karti hain), aur Tight Coupling se bacha jaata hai.

---

## 📝 Lecture 01 Summary (Quick Revision)

- **LLD focus**: Code structure, class diagrams, clean architecture, design principles (SOLID), and design patterns.
- **Class vs Object**: Class is the blueprint (no memory allocated for state), Object is the living entity in memory.
- **State + Behavior**: Real world entities ko code mein model karne ke liye unka data (variables) aur action (methods) ek unit mein bind kiye jaate hain.
- **Interview Goal**: Interviewer ye dekhta hai ki kya aap clean, scalable, decoupled aur readable code likh sakte ho jo requirements badalne par easily adapt ho sake.
