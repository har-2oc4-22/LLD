# Practice Problems in Low-Level Design

---

## 🎯 Overview

Is directory mein real-world low-level design practice problem sets included hain:
- [Practice Problem 1 (PDF)](./PracticeProblem1.pdf)
- [Practice Problem 2 (PDF)](./PracticeProblem2.pdf)

---

## 💡 Recommended Problem-Solving Steps

Jab bhi aap koi naya LLD problem solve karein, in steps ko follow karein:

1. **Step 1: Problem Definition**: Core problem statement ko apne words mein summarize karein.
2. **Step 2: Actors & User Stories**: Kaunse actors system ke sath interact karte hain? (e.g. Rider, Driver, Admin).
3. **Step 3: Class Diagram (UML)**: Pen aur paper par entities, methods, aur relationships (Association, Aggregation, Composition, Inheritance) draw karein.
4. **Step 4: Design Patterns**: Dekhein kahan patterns fit hote hain (e.g., matching algorithm ke liye Strategy, status updates ke liye Observer).
5. **Step 5: Code Implementation**: Pehle Interfaces aur Abstract classes define karein, fir concrete classes implement karein.

---

## 📝 Machine Coding Best Practices

- Don't hardcode values; use constants or enums.
- Use meaningful exception types instead of generic `RuntimeException`.
- Ensure methods are small (< 20 lines) and have a single clear responsibility.
