# Lecture 22: Chain of Responsibility (COR) Design Pattern

---

## 🎯 What is Chain of Responsibility?

> **Definition**: *"Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it."*

Jab kisi request ko process karne ke liye ek sequential **Chain of Handlers** hoti hai, jahan har handler ya toh request ko poori tarah process karta hai, ya partially handle karke bacha hua kaam **Next Handler** ko pass kar deta hai.

---

## 💡 Real-World Analogy: ATM Cash Dispenser 🏧 & Customer Care Escalation 📞

### 1. ATM Cash Dispenser (Lecture Code)
Jab aap ATM se **₹4,300** nikalte ho:
- **₹1000 Handler**: 4 notes ₹1000 ke dispense karta hai (₹4000). Remaining bacha ₹300. Ye ₹300 next handler ko pass karta hai.
- **₹500 Handler**: ₹300 ke liye 0 notes dispense karta hai. Pass to next.
- **₹200 Handler**: 1 note ₹200 ka dispense karta hai (₹200). Remaining bacha ₹100. Pass to next.
- **₹100 Handler**: 1 note ₹100 ka dispense karta hai. Remaining = 0. Transaction Successful!

### 2. Tech Support Escalation
- Level 1: Chatbot (FAQs solve karta hai). Agar nahi hua ->
- Level 2: Human Support Agent. Agar complex technical bug hai ->
- Level 3: Core Engineering Team.

---

## 🏗️ Architecture: Linked Handlers Chain

```text
[Client] ── sends request ──> [Handler A]
                                  │ passes if unhandled
                                  ▼
                              [Handler B]
                                  │ passes if unhandled
                                  ▼
                              [Handler C]
```

---

## 💻 Code Walkthrough

```java
// 1. Base Handler with nextHandler pointer (Like a Linked List Node!)
abstract class MoneyHandler {
    protected MoneyHandler nextHandler;

    public void setNextHandler(MoneyHandler next) {
        this.nextHandler = next;
    }

    public abstract void dispense(int amount);
}

// 2. Concrete Handler
class FiveHundredHandler extends MoneyHandler {
    private int numNotes;

    public FiveHundredHandler(int notes) { this.numNotes = notes; }

    @Override
    public void dispense(int amount) {
        int notesNeeded = amount / 500;
        int dispensed = Math.min(notesNeeded, numNotes);
        numNotes -= dispensed;

        if (dispensed > 0) {
            System.out.println("Dispensing " + dispensed + " x ₹500 notes");
        }

        int remaining = amount - (dispensed * 500);

        // Forward to the next link in the chain
        if (remaining > 0 && nextHandler != null) {
            nextHandler.dispense(remaining);
        }
    }
}
```

### 3. Setting Up the Chain (Client Code)
```java
MoneyHandler h1000 = new ThousandHandler(5);
MoneyHandler h500  = new FiveHundredHandler(10);
MoneyHandler h100  = new HundredHandler(20);

// Building the chain
h1000.setNextHandler(h500);
h500.setNextHandler(h100);

// Triggering request at chain head
h1000.dispense(3700);
```

---

## 🚀 Famous Real-World Usages of COR

1. **HTTP Middlewares & Filters**: Express.js (`app.use((req, res, next) => ...)`), Spring Boot Security Filters (`FilterChain`), ASP.NET Core Middleware.
2. **Logging Frameworks**: Log4j / Logback filtering (DEBUG -> INFO -> WARN -> ERROR).
3. **UI Event Bubbling**: JavaScript DOM event bubbling (Button -> Div -> Body -> Window).

---

## 📝 Lecture 22 Summary (Quick Revision)

- **Sender-Receiver Decoupling**: Client ko sirf chain ke pehle handler ko call karna hota hai, client ko aage ki chain structure ki chinta nahi hoti.
- **Dynamic Reordering**: Handlers ka order runtime par easily change kiya ja sakta hai bina handlers ka code change kiye.
- **Single Responsibility**: Har handler sirf apne specific kaam (e.g. ₹500 notes ya Authentication check) ke liye responsible hota hai.
