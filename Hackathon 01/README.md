# Hackathon 01: Machine Coding Challenge

---

## 🎯 Overview

Is folder mein LLD course ka **Hackathon 01** problem statement include kiya gaya hai ([Problem Statement.pdf](./Problem%20Statement.pdf)).

---

## 💡 How to Approach a Machine Coding Hackathon

Machine Coding hackathons standard company hiring drives (jaise Flipkart, Swiggy, Uber, Cred, BrowserStack) ke 90-120 minute design rounds ko simulate karte hain:

1. **Understand Requirements**: Problem statement ko dhyan se padhein aur core entities identify karein.
2. **Modular Architecture**: Code ko multi-file package structure mein design karein:
   - `models/`: Data classes (e.g. User, Account, Product).
   - `services/` ya `managers/`: Core business logic classes.
   - `strategies/`: Interchangeable algorithms (pricing, allocation, matching).
   - `enums/`: State types, status codes, action types.
3. **In-Memory Storage**: Production database ki jagah in-memory collections (`Map<String, Model>`, `List<Model>`) use karein with thread-safe data structures (`ConcurrentHashMap`) jahan concurrency required ho.
4. **Driver Code (`Main`)**: Har corner case aur user flow ko demonstrate karne ke liye robust `main()` method likhein.

---

## 📝 Key Evaluation Criteria

- **Correctness**: Kya saari functional requirements execute ho rahi hain?
- **Extensibility**: Agar interviewer round ke end mein naya requirement add kare, toh kya code bina pure system ko modify kiye adapt ho sakta hai?
- **Code Cleanliness**: Meaningful variable/method names, proper indentation, aur SOLID principles ka adherence.
