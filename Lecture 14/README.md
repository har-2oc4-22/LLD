# Lecture 14: Machine Coding Case Study - Multi-Channel Notification System

---

## 🎯 System Overview

Is case study mein ek production-grade **Multi-Channel Notification System** design kiya gaya hai jo ek sath **4 major design patterns** ko beautifully combine karta hai:
1. **Singleton Pattern**: Centralized `NotificationService`.
2. **Decorator Pattern**: Dynamic message enrichment (Timestamps, Signatures, Formatting).
3. **Observer Pattern**: Event notification pipeline (`Logger`, `NotificationEngine`).
4. **Strategy Pattern**: Multi-channel delivery adapters (Email, SMS, In-App Popup).

---

## 📋 Functional Requirements

1. **Dynamic Content Formatting**: Notification text ke upar dynamically metadata (Timestamp, Corporate Signature, Encryption) decorate kiya ja sake bina base text class badle.
2. **Event Broadcasting**: Jab bhi koi nayi notification generate ho, sabhi registered listeners (Auditing Logger, Delivery Engine) automatically notify ho jayein.
3. **Multi-Channel Dispatch**: Ek notification ek ya ek se zyada delivery channels (Email, SMS, Push Notification, WhatsApp) par dispatch ho sake.
4. **Central Management**: Poori application ke paas single centralized dispatch point ho.

---

## 🏗️ 4-Pattern Architecture Breakdown

```text
[Client / Tomato App]
        │
        ▼
[NotificationService] (Singleton)
        │ has-a
[NotificationObservable] (Observer Pattern: Subject)
        │ notifies observers
        ├── [Logger] (Observer 1: Audit logs)
        └── [NotificationEngine] (Observer 2: Dispatcher)
                 │ has multiple strategies
                 ├── [EmailStrategy] (Strategy 1)
                 ├── [SMSStrategy]   (Strategy 2)
                 └── [PopUpStrategy] (Strategy 3)

[INotification] ── decorated by ──> [TimestampDecorator] ──> [SignatureDecorator]
```

---

## 💻 Code Walkthrough

### 1. Decorating the Content
```java
// Base notification message
INotification notification = new SimpleNotification("Your order #1042 has been shipped!");

// Wrap dynamically with timestamp and signature
notification = new TimestampDecorator(notification);
notification = new SignatureDecorator(notification, "Swiggy Customer Support Team");

System.out.println(notification.getContent());
// Result:
// [2026-11-19 15:30:00] Your order #1042 has been shipped!
// -- Swiggy Customer Support Team
```

### 2. Multi-Channel Strategy Dispatch
```java
NotificationEngine engine = new NotificationEngine();

// Add desired delivery channels
engine.addNotificationStrategy(new EmailStrategy("customer@gmail.com"));
engine.addNotificationStrategy(new SMSStrategy("+91 9876543210"));
engine.addNotificationStrategy(new PopUpStrategy());
```

### 3. Triggering via Central Singleton Service
```java
NotificationService service = NotificationService.getInstance();
service.sendNotification(notification); 
// 1. Logger prints audit trail
// 2. Email dispatched
// 3. SMS dispatched
// 4. In-App popup displayed
```

---

## 📝 Lecture 14 Summary (Quick Revision)

- **Power of Composition**: Kaise 4 alag-alag design patterns ek cohesive system banane ke liye collaborate karte hain.
- **Decorator**: Content styling aur security wrappers ke liye.
- **Observer**: Decoupling event generation from event handling.
- **Strategy**: Decoupling channel communication protocols from the engine.
