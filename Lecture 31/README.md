# Lecture 31: Machine Coding Case Study - Splitwise (Expense Sharing & Debt Simplification)

---

## 🎯 System Overview

Is case study mein **Splitwise** ka complete Low-Level Design cover kiya gaya hai.
Yeh project interviewers ka absolute favorite machine coding problem hai kyunki isme:
1. **Complex Calculation Strategies**: Equal Split, Exact Amount Split, aur Percentage (%) Split.
2. **Debt Simplification Algorithm**: Minimum cash-flow algorithm to reduce total payment transactions among group members.
3. **Observer Notifications**: Jab bhi koi expense add ho, affected members ko notify karna.
4. **Group & Non-Group Balances**: Per-friend ledger and group ledger maintenance.

---

## 📋 Functional Requirements

1. **User & Group Operations**:
   - Users can create groups and add friends.
2. **Expense Creation with Diverse Splits**:
   - **EQUAL**: Total amount divided equally among all participants.
   - **EXACT**: Specific currency amounts specified per participant ($\sum \text{exact} = \text{total}$).
   - **PERCENT**: Percentages specified per participant ($\sum \% = 100\%$).
3. **Balance Sheet**:
   - Show how much a user owes or is owed by each friend.
4. **Simplify Debts Feature**:
   - Reduces cyclical / transitive debts:
   - *Example*: A owes B ₹100, B owes C ₹100 $\implies$ **A directly pays C ₹100** (2 transactions reduced to 1!).

---

## 🧮 The Debt Simplification Algorithm (Min Cash-Flow)

```text
Without Simplification:
  Bob ── owes ₹200 ──> Alice
  Charlie ── owes ₹200 ──> Bob
  David ── owes ₹100 ──> Charlie
  (Total: 3 transactions)

Net Balances:
  Alice:   +₹200 (Creditor)
  Bob:        ₹0 (Settled)
  Charlie: -₹100 (Debtor)
  David:   -₹100 (Debtor)

With Simplification:
  Charlie ── pays ₹100 ──> Alice
  David   ── pays ₹100 ──> Alice
  (Total: 2 direct transactions, Bob eliminated!)
```

### Algorithm Steps:
1. Har user ka **Net Amount** calculate karo:
   $$\text{Net}[u] = \sum \text{Owed to } u - \sum u \text{ Owes}$$
2. Users ko do lists mein separate karo:
   - **Creditors**: $\text{Net} > 0$ (jinko paise milne hain).
   - **Debtors**: $\text{Net} < 0$ (jinko paise dene hain).
3. Max creditor aur Max debtor ko match karo:
   - $\text{Amount} = \min(\text{credit}, \text{debt})$.
   - Debtor directly creditor ko settle karega.
   - Ek party ka balance 0 ho kar eliminate ho jayega. Repeat until all 0.

---

## 🏗️ Design Patterns Applied

| Pattern | Component | Why Chosen |
| :--- | :--- | :--- |
| **Strategy Pattern** | `SplitStrategy` (`EqualSplit`, `ExactSplit`, `PercentageSplit`) | Split calculation aur validation algorithms ko interchangeable banata hai. |
| **Factory Pattern** | `SplitFactory` | `SplitType` enum ke hisaab se correct split strategy create karta hai. |
| **Observer Pattern** | `Observer`, `User` | Jab expense register hota hai, affected users ko real-time update push hoti hai. |
| **Facade Pattern** | `Splitwise` | Client ke liye `addExpense()`, `showBalances()`, aur `simplifyDebts()` ka clean API. |

---

## 💻 Code Walkthrough

```java
// 1. Split Strategy Interface
interface SplitStrategy {
    void calculateSplit(Expense expense, double totalAmount, List<User> users, List<Double> values);
    boolean validate(double totalAmount, List<Double> values);
}

// 2. Client Flow via Facade
Splitwise app = new Splitwise();

User u1 = app.registerUser("u1", "Alice", "alice@test.com");
User u2 = app.registerUser("u2", "Bob", "bob@test.com");
User u3 = app.registerUser("u3", "Charlie", "charlie@test.com");

Group goaTrip = app.createGroup("g1", "Goa Trip", Arrays.asList(u1, u2, u3));

// Add Expense: Alice paid ₹3000 split equally
app.addExpense("e1", "Dinner", 3000, u1, Arrays.asList(u1, u2, u3), SplitType.EQUAL, null, "g1");

// View balances before and after simplification
app.showBalances("g1");
app.simplifyDebts("g1");
```

---

## 📝 Lecture 31 Summary (Quick Revision)

- **Greedy Settlement**: Net-balance array ko balance karke $O(N^2)$ transactions ko maximum $N-1$ transactions mein convert kiya ja sakta hai.
- **Precision Validation**: Exact split aur Percentage split mein validation hamesha zaroori hai (Floating point precision errors prevent karne ke liye).
- **Core Entities**: `User`, `Group`, `Expense`, `Split`, `SplitStrategy`, `DebtSimplifier`.
