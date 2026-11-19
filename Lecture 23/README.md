# Lecture 23: Machine Coding Case Study - Payment Gateway System (Razorpay / Stripe)

---

## 🎯 System Overview

Is case study mein ek robust **Payment Gateway Integration Engine** (Razorpay / Stripe style) ka Low-Level Design cover kiya gaya hai.
System validate karta hai payments ko, banking APIs ke sath initiate karta hai, confirm karta hai, aur network failures ke case mein **Automatic Retries via Proxy** handle karta hai.

---

## 📋 Functional Requirements

1. **Standard Payment Pipeline**: Har payment ko 3 mandatory steps follow karne hote hain:
   - `validatePayment()` (Currency, positive amount check).
   - `initiatePayment()` (Communication with bank).
   - `confirmPayment()` (Receipt / settlement confirmation).
2. **Multiple Banking Gateways**: Paytm, Razorpay, etc. with different success rates and rules.
3. **Resilience & Automatic Retries**: Agar bank server se transient network error aaye, toh client code ko error return karne se pehle system automatically 3 times retry kare.
4. **Single Gateway Controller**: Client code ek centralized controller ko call kare bina gateway ke internal architecture ko jaane.

---

## 🏗️ Design Patterns Applied

| Pattern | Component | Why Chosen |
| :--- | :--- | :--- |
| **Template Method** | `PaymentGateway` | Standard 3-step payment sequence (`validate` $\to$ `initiate` $\to$ `confirm`) ko enforce karta hai; concrete gateways steps ko customize karti hain. |
| **Proxy Pattern** | `PaymentGatewayProxy` | Real gateway ko wrap karke transparent **Auto-Retry mechanism** provide karta hai bina core payment logic ko touch kiye. |
| **Strategy Pattern** | `BankingSystem` | Bank API processing logic ko switchable banata hai. |
| **Factory + Singleton** | `GatewayFactory`, `PaymentController` | Pre-configured gateway proxies instantiate karta hai aur single entry point provide karta hai. |

---

## 💻 Code Walkthrough

### 1. Template Method (Enforcing the Sequence)
```java
abstract class PaymentGateway {
    // Template method: Fixed pipeline
    public boolean processPayment(PaymentRequest request) {
        if (!validatePayment(request)) return false;
        if (!initiatePayment(request)) return false;
        if (!confirmPayment(request))  return false;
        return true;
    }

    protected abstract boolean validatePayment(PaymentRequest req);
    protected abstract boolean initiatePayment(PaymentRequest req);
    protected abstract boolean confirmPayment(PaymentRequest req);
}
```

### 2. Resilience Proxy (Retry Logic)
```java
class PaymentGatewayProxy extends PaymentGateway {
    private PaymentGateway realGateway;
    private int maxRetries;

    public PaymentGatewayProxy(PaymentGateway realGateway, int retries) {
        this.realGateway = realGateway;
        this.maxRetries = retries;
    }

    @Override
    public boolean processPayment(PaymentRequest request) {
        for (int attempt = 1; attempt <= maxRetries; attempt++) {
            boolean success = realGateway.processPayment(request);
            if (success) return true;
            System.out.println("Retry attempt " + attempt + " failed...");
        }
        return false;
    }
    // ... delegates validation & confirmation to realGateway
}
```

---

## 📝 Lecture 23 Summary (Quick Revision)

- **Template Method for Security**: Enforces that no gateway can accidentally bypass validation before initiation.
- **Proxy for Non-Functional Requirements**: Retries, rate-limiting, and metric logging belong in a Proxy layer, not in the core business logic.
- **Pluggability**: Naya gateway (e.g. `CashfreeGateway`) integrate karna sirf naye subclass aur factory update ki baat hai.
