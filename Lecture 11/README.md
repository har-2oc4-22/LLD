# Lecture 11: Machine Coding Case Study - Tomato (Food Delivery System)

---

## 🎯 System Overview

**Tomato** ek complete end-to-end food ordering and delivery system (Zomato / Swiggy Clone) ka Low-Level Design hai.
Isme restaurant browsing, cart management, multiple order types (Delivery vs Pickup), order scheduling (Now vs Scheduled), multiple payment strategies (UPI, Card), aur notifications ko modular architecture mein design kiya gaya hai.

---

## 📋 Functional Requirements

1. **User & Restaurant Management**:
   - Multiple restaurants register ho sakte hain with their unique menus (`MenuItem`).
   - User kisi bhi restaurant ka menu browse kar sakta hai.
2. **Cart Operations**:
   - User items cart mein add kar sakta hai, quantity change kar sakta hai, total calculate kar sakta hai.
3. **Flexible Ordering Types**:
   - **Delivery Order**: Requires delivery address, assigns delivery partner, delivery fee included.
   - **Pickup Order**: User directly restaurant counter se pick karega.
4. **Order Scheduling**:
   - **Now Order**: Instant preparation and dispatch.
   - **Scheduled Order**: Future delivery time slot set kiya jaata hai.
5. **Pluggable Payment Modes**:
   - Support for UPI, Credit Card, etc.
6. **Notification Alerts**:
   - Order confirmation par customer aur restaurant dono ko notification send hoti hai.

---

## 🏗️ Design Patterns Applied

| Pattern | Where it is Used | Why it was Chosen |
| :--- | :--- | :--- |
| **Facade Pattern** | `TomatoApp` | Client ke liye ek single unified interface provide karta hai taaki client ko alag-alag managers (RestaurantManager, OrderManager) se manually deal na karna pade. |
| **Abstract Factory** | `OrderFactory` (`NowOrderFactory`, `ScheduledOrderFactory`) | Now Delivery, Now Pickup, Scheduled Delivery, aur Scheduled Pickup orders ko cleanly instantiate karta hai. |
| **Strategy Pattern** | `PaymentStrategy` (`UpiPaymentStrategy`, `CreditCardPaymentStrategy`) | Payment methods ko interchangeable aur extendable banata hai bina Order class ko modify kiye. |
| **Manager Pattern** | `RestaurantManager`, `OrderManager` | In-memory data collections ko encapsulate karta hai aur CRUD business operations handle karta hai. |

---

## 🗂️ Package Structure

```text
Tomato/
├── TomatoApp.java                # Facade (Main orchestrator)
├── Main.java                     # Driver code
├── models/
│   ├── User.java                 # Customer details
│   ├── Restaurant.java           # Restaurant details & menus
│   ├── MenuItem.java             # Food item, price
│   ├── Cart.java                 # Shopping cart & totals
│   ├── Order.java                # Abstract base order
│   ├── DeliveryOrder.java        # Order with delivery address
│   └── PickupOrder.java          # Order for counter pickup
├── managers/
│   ├── RestaurantManager.java    # Manages list of restaurants
│   └── OrderManager.java         # Stores & tracks active orders
├── factories/
│   ├── OrderFactory.java         # Abstract order factory
│   ├── NowOrderFactory.java      # Factory for immediate orders
│   └── ScheduledOrderFactory.java# Factory for scheduled orders
├── strategies/
│   ├── PaymentStrategy.java      # Base payment contract
│   ├── UpiPaymentStrategy.java   # UPI processing
│   └── CreditCardPaymentStrategy.java # Card processing
└── services/
    └── NotificationService.java  # Alert dispatch service
```

---

## 💻 Key Execution Flow (Client Usage)

```java
// 1. Initialize Application Facade
TomatoApp app = new TomatoApp();

// 2. Add Restaurants and Menu
Restaurant r1 = new Restaurant("Bawarchi", "Hyderabad");
r1.addMenuItem(new MenuItem("Biryani", 250));
app.addRestaurant(r1);

// 3. Customer builds Cart
Cart cart = new Cart();
cart.addItem(r1.getMenuItem("Biryani"), 2);

// 4. Place Order via Factory and Strategy
OrderFactory factory = new NowOrderFactory();
PaymentStrategy payment = new UpiPaymentStrategy("user@upi");

Order order = app.placeOrder(user, cart, factory, payment, OrderType.DELIVERY);
```

---

## 📝 Lecture 11 Summary (Quick Revision)

- **Separation of Concerns**: Data models (Cart, User), Business logic (Managers), Object Creation (Factories), aur Algorithms (Payment Strategies) completely decoupled hain.
- **Extensibility**: Kal ko naya payment method (e.g. NetBanking, Crypto) ya naya order type (e.g. DineInOrder) add karna ho, toh 0 existing code change hoga (OCP compliant).
- **Clean Facade**: `TomatoApp` acts as the front door, making client interaction seamless.
