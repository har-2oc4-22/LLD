# Lecture 26: Machine Coding Case Study - Zepto Clone (10-Minute Quick Commerce)

---

## 🎯 System Overview

Is case study mein **Zepto / Blinkit / Instamart** jaise 10-minute Quick Commerce delivery platform ka Low-Level Design cover kiya gaya hai.
System solve karta hai:
- **DarkStore (Micro-Warehouse) Network**: Har DarkStore ke paas apni geo-location `(x, y)` aur live inventory hoti hai.
- **Proximity Search**: User ki location se 5 KM radius ke andar ke DarkStores ko distance ke hisaab se sort karna.
- **Multi-Store Order Splitting**: Agar sabse paas wale DarkStore mein saara stock available nahi hai, toh order ko multiple stores mein split karke multiple delivery partners assign karna!
- **Automated Inventory Replenishment**: Threshold-based stock refilling strategy.

---

## 📋 Key Algorithms & Flows

### 1. Proximity DarkStore Discovery
User coordinates $(u_x, u_y)$ aur DarkStore coordinates $(s_x, s_y)$ ke beech Euclidean distance calculate hota hai:
$$d = \sqrt{(s_x - u_x)^2 + (s_y - u_y)^2}$$
Stores within threshold ($d \le 5.0\text{ KM}$) sort hokar priority list bante hain.

### 2. Multi-Store Stock Fulfillment Algorithm
```text
Customer Cart: [Apple x4, Banana x3, Chocolate x2]
   │
   ├── Check Closest Store (DarkStore A)
   │     └── Has: Apple x5, Banana x2, Chocolate x0
   │           - Takes: Apple x4, Banana x2
   │           - Remaining needed: Banana x1, Chocolate x2
   │
   └── Check Next Store (DarkStore B)
         └── Has: Chocolate x10
               - Takes: Chocolate x2
               - Remaining needed: Banana x1
   │
   └── Check Next Store (DarkStore C)
         └── Has: Banana x5
               - Takes: Banana x1 (Order 100% fulfilled!)
   │
Result: 3 Delivery Partners dispatched from 3 mini-warehouses for 10-minute delivery!
```

---

## 🏗️ Architecture & Patterns

- **Strategy Pattern (`ReplenishStrategy`)**: `ThresholdReplenishStrategy` (jab stock < 3 ho toh refill karo) vs `WeeklyReplenishStrategy`.
- **Factory Pattern (`ProductFactory`)**: Product SKU lookup and instantiation.
- **Singleton Pattern (`DarkStoreManager`, `OrderManager`)**: Central registry for all DarkStores across the city.
- **Composite Inventory (`DbInventoryStore`, `InventoryManager`)**: Clean CRUD separation for warehouse stock.

---

## 📝 Lecture 26 Summary (Quick Revision)

- **DarkStore Concept**: Decentralized micro-warehouses close to consumer clusters enable sub-10 minute deliveries.
- **Graceful Order Splitting**: Single store par stock out hone par order cancel nahi hota, balki multi-store routing se fulfill hota hai.
- **Euclidean Geo-filtering**: Proximity calculations filter stores efficiently before stock verification.
