# Lecture 05: SOLID Principles (Part 1 - S, O, L)

---

## 🎯 What is SOLID?

SOLID 5 design principles ka ek acronym hai jo Robert C. Martin (Uncle Bob) ne diye the. Inka maksad hota hai:
- Code ko **maintainable**, **extensible**, aur **loosely coupled** banana.
- Naye feature aane par existing code ko todne (regression bugs) se bachana.

---

## 1. 🎯 Single Responsibility Principle (SRP)

> **Golden Rule**: *"A class should have ONE, and only ONE, reason to change."*

### 💡 Real-World Analogy
Agar ek hi banda Restaurant mein Chef bhi hai, Waiter bhi hai aur Accountant bhi hai:
- Agar menu change hua toh wo pareshan.
- Agar billing software change hua toh wo pareshan.
- Agar customer handle karne ka tareeqa badla toh wo pareshan.
**Behtar solution:** Chef sirf khana banaye, Waiter sirf serve kare, aur Accountant sirf hisaab rakhe!

### ❌ Violation (Bad Code)
```java
class ShoppingCart {
    List<Product> products;
    
    double calculateTotal() { /* Calculation logic */ }
    void printInvoice() { /* Formatting and printing logic */ }
    void saveToDatabase() { /* SQL DB connection logic */ }
}
```
*Problem*: Agar DB change hua (e.g., MySQL se MongoDB), ya receipt ka layout badla, ya GST rate badla — har baar `ShoppingCart` modify karni padegi.

### ✅ Followed (Clean Code)
```java
// 1. Only Cart business logic
class ShoppingCart {
    double calculateTotal() { ... }
}

// 2. Only Presentation logic
class ShoppingCartPrinter {
    void printInvoice(ShoppingCart cart) { ... }
}

// 3. Only Persistence logic
class ShoppingCartStorage {
    void saveToDatabase(ShoppingCart cart) { ... }
}
```

---

## 2. 🚪 Open/Closed Principle (OCP)

> **Golden Rule**: *"Software entities should be OPEN for extension, but CLOSED for modification."*

### 💡 Real-World Analogy
Aapke phone mein 3.5mm headphone jack ya Type-C port hota hai. Naye headphones lene ke liye aapko apna phone tod kar motherboard nahi modify karna padta. Interface universal hai — naya device plug-in karo aur chalao.

### ❌ Violation (Bad Code)
```java
class ShoppingCartStorage {
    void save(String dbType) {
        if (dbType.equals("SQL")) {
            // save to SQL
        } else if (dbType.equals("MongoDB")) {
            // save to Mongo
        } else if (dbType.equals("File")) {
            // save to File
        }
    }
}
```
*Problem*: Har baar naya DB aane par existing tested class ke andar ghuskar `if-else` add karna padega (Risk of bugs in existing code!).

### ✅ Followed (Clean Code)
```java
interface Persistence {
    void save(ShoppingCart cart);
}

class SQLPersistence implements Persistence {
    public void save(ShoppingCart cart) { /* SQL Logic */ }
}

class MongoPersistence implements Persistence {
    public void save(ShoppingCart cart) { /* Mongo Logic */ }
}

class FilePersistence implements Persistence {
    public void save(ShoppingCart cart) { /* File Logic */ }
}
```
*Advantage*: Naya storage (e.g., `RedisPersistence`) add karne ke liye bas ek nayi class banao jo `Persistence` implement kare. Purana code touch bhi nahi karna padega!

---

## 3. 🔄 Liskov Substitution Principle (LSP)

> **Golden Rule**: *"Subtypes must be substitutable for their base types without altering the correctness of the program."*

### 💡 Real-World Analogy: Electric vs Petrol Toy Car
Agar aap kisi remote-controlled car mein Normal AA batteries ki jagah Rechargeable AA batteries lagate ho, toh car ko pata bhi nahi chalna chahiye aur bina crash huye normal chalni chahiye. Lekin agar battery lagane par car bole *"I don't support recharge, boom!"* toh LSP toot gaya!

### ❌ Classic Violation (The Fixed Deposit Trap)
```java
class BankAccount {
    void deposit(double amount) { ... }
    void withdraw(double amount) { ... }
}

class FixedTermAccount extends BankAccount {
    @Override
    void withdraw(double amount) {
        // Fixed deposit se instantly withdraw nahi ho sakta!
        throw new UnsupportedOperationException("Cannot withdraw from Fixed Term!");
    }
}
```
*Problem*: Agar client code loop chala raha hai:
```java
for (BankAccount acc : accounts) {
    acc.withdraw(100); // FixedTermAccount aate hi system crash ho jayega!
}
```

### ✅ Followed (Clean Code)
Interface ko hierarchy ke hisaab se segregate karo:

```java
interface DepositOnlyAccount {
    void deposit(double amount);
}

interface WithdrawableAccount extends DepositOnlyAccount {
    void withdraw(double amount);
}

class SavingAccount implements WithdrawableAccount { ... }
class CurrentAccount implements WithdrawableAccount { ... }
class FixedTermAccount implements DepositOnlyAccount { ... } // Only deposit allowed!
```
Ab client kabhi bhi `FixedTermAccount` par `withdraw()` call hi nahi kar payega kyunki type system compile time par hi invalid operations rok lega.

---

## 📝 Lecture 05 Summary (Quick Revision)

- **SRP**: Har class ka ek hi single goal/responsibility hona chahiye (Separation of Concerns).
- **OCP**: Naya feature = Nayi class (Inheritance / Interface implementation), kabhi bhi existing working code ke `if-else` ko modify mat karo.
- **LSP**: Child class ko parent class ki jagah safely replace kiya ja sake bina program ko crash ya unexpected state mein daale.
