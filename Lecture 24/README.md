# Lecture 24: Machine Coding Case Study - Discount & Coupon Engine

---

## 🎯 System Overview

Is case study mein eCommerce platforms (Amazon / Flipkart / Myntra) ka core **Discount & Coupon Evaluation Engine** design kiya gaya hai.
System support karta hai:
- Multiple discount calculation algorithms (Flat off, Percentage off, Percentage with maximum cap).
- Category-specific discounts (Seasonal offers on clothing).
- Cart-level discounts (Bulk purchase, Loyalty member discount, Partner Bank credit card discount).
- Coupon chaining via **Chain of Responsibility**.
- Thread-safe coupon evaluation using `ReentrantLock`.

---

## 📋 Functional Requirements

1. **Diverse Discount Strategies**:
   - Flat Discount: e.g. Flat ₹100 off.
   - Percentage Discount: e.g. 10% off.
   - Capped Percentage: e.g. 15% off up to maximum ₹500.
2. **Eligibility Rules**:
   - Minimum cart threshold check.
   - Category-matching rules.
   - Payment method (Bank partner) eligibility.
   - Loyalty membership check.
3. **Combinability & Chaining**:
   - Coupons can be sequentially applied in a pipeline (Chain of Responsibility) where each discount applies to the remaining cart subtotal.
4. **Thread Safety**:
   - Highly concurrent environment mein coupon registration aur evaluation synchronized rahe.

---

## 🏗️ Design Patterns Applied

| Pattern | Component | Why Chosen |
| :--- | :--- | :--- |
| **Strategy Pattern** | `DiscountStrategy` | Math calculation (Flat vs % vs % with Cap) ko cart rules se decouple karta hai. |
| **Chain of Responsibility** | `Coupon` (`SeasonalOffer`, `LoyaltyDiscount`, `BankingCoupon`) | Coupons ko sequential pipeline mein chain karta hai jahan ek coupon apply hokar cart next coupon ko pass karta hai. |
| **Singleton + Locking** | `CouponManager`, `ReentrantLock` | Centralized registry for all platform coupons with thread-safe evaluation. |

---

## 💻 Code Walkthrough

```java
// 1. Discount Calculation Strategy
interface DiscountStrategy {
    double calculate(double baseAmount);
}

class PercentageWithCapStrategy implements DiscountStrategy {
    private double percent, cap;
    public double calculate(double base) {
        double d = (percent / 100.0) * base;
        return Math.min(d, cap); // Cannot exceed cap!
    }
}

// 2. Coupon Chain Element
abstract class Coupon {
    private Coupon next;
    public void setNext(Coupon n) { this.next = n; }

    public void applyDiscount(Cart cart) {
        if (isApplicable(cart)) {
            double discount = getDiscount(cart);
            cart.applyDiscount(discount);
            System.out.println(name() + " applied: ₹" + discount);
            if (!isCombinable()) return; // Stop chain if non-combinable
        }
        if (next != null) next.applyDiscount(cart); // Forward to next coupon
    }
}
```

---

## 📝 Lecture 24 Summary (Quick Revision)

- **Composability**: Chain of Responsibility allows multiple promotions to stack gracefully or terminate if non-combinable.
- **Rule Separation**: Applicability logic (`isApplicable`) is separated from mathematical calculation (`DiscountStrategy`).
- **Thread Safety**: `ReentrantLock` protects coupon management under high-traffic flash sale scenarios.
