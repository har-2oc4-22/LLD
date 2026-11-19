# Lecture 32: State Design Pattern (Vending Machine System)

---

## 🎯 What is State Design Pattern?

> **Definition**: *"Allow an object to alter its behavior when its internal state changes. The object will appear to change its class."*

State pattern finite state machines (FSM) ko implement karne ka sabse clean object-oriented tareeqa hai. Har state ko ek independent class bana diya jaata hai, aur context object apni current state object ko actions delegate kar deta hai.

---

## 💡 Real-World Analogy: Vending Machine 🥤

Ek Vending machine ke alag-alag states hote hain:
1. **No Coin State**: User se coin expect karta hai. Agar user item select karega toh bolega *"Pehle coin daalo!"*.
2. **Has Coin State**: Coin aa chuka hai. User ya toh aur coins daal sakta hai, item choose kar sakta hai, ya coin return maang sakta hai.
3. **Dispense State**: Machine product drop kar rahi hai. Is dauran koi naya coin accept nahi hoga.
4. **Sold Out State**: Saare items khatam ho chuke hain. Koi bhi coin daalega toh instant refund ho jayega.

---

## ❌ The Anti-Pattern: Spaghetti `switch-case` / `if-else`

Agar aap bina State pattern ke code likhoge:
```java
void selectItem() {
    if (state == NO_COIN) {
        System.out.println("Insert coin first!");
    } else if (state == HAS_COIN) {
        if (balance >= price) state = DISPENSING;
    } else if (state == DISPENSING) {
        System.out.println("Wait, already dispensing!");
    } else if (state == SOLD_OUT) {
        System.out.println("Machine is sold out!");
    }
}
```
*Problem*: Aise hi `insertCoin()`, `dispense()`, `returnCoin()`, `refill()` sabhi methods mein 4-5 if-else blocks honge. Kal ko ek naya state (e.g. `MaintenanceState`) add karna ho toh **saari 10 methods modify karni padengi (High Risk of Bugs!)**.

---

## 🏗️ The Clean Architecture (State Pattern)

```text
               [VendingState] (State Interface)
                 ├── insertCoin()
                 ├── selectItem()
                 ├── dispense()
                 └── returnCoin()
                 /     |      |     \
    [NoCoinState] [HasCoin] [Dispense] [SoldOutState]
          ▲
          │ delegates action & holds current state
   [VendingMachine] (Context)
```

---

## 💻 Code Walkthrough

```java
// 1. State Interface
interface VendingState {
    VendingState insertCoin(VendingMachine machine, int coin);
    VendingState selectItem(VendingMachine machine);
    VendingState dispense(VendingMachine machine);
    VendingState returnCoin(VendingMachine machine);
}

// 2. Concrete State: NoCoinState
class NoCoinState implements VendingState {
    public VendingState insertCoin(VendingMachine machine, int coin) {
        machine.setInsertedCoin(coin);
        System.out.println("Coin inserted: Rs " + coin);
        return machine.getHasCoinState(); // State Transition!
    }

    public VendingState selectItem(VendingMachine machine) {
        System.out.println("Please insert coin first!");
        return this; // Stay in same state
    }
    // ...
}

// 3. Context Class: VendingMachine
class VendingMachine {
    private VendingState currentState;

    public void insertCoin(int coin) {
        // Delegates action to current state and updates current state with returned state
        currentState = currentState.insertCoin(this, coin);
    }
}
```

---

## 🌟 Benefits of State Pattern

1. **SRP (Single Responsibility)**: Har state ka behavior uski apni dedicated class mein isolated rehta hai.
2. **OCP (Open/Closed Principle)**: Naya state (e.g., `CardPaymentState`) add karne ke liye existing states ke code ko bina chhue bas nayi state class implement karo.
3. **No Flag Variables**: Code se messy boolean flags (`isCoinInserted`, `isDispensing`, etc.) completely khatam ho jaate hain.

---

## 📝 Lecture 32 Summary (Quick Revision)

- **Behavior Depends on State**: Same method call (`selectItem()`) alag-alag output deti hai depending on whether coin is inserted or not.
- **State as First-Class Objects**: States are represented as classes, not enum/integer primitives.
- **Clean Transitions**: State transition logic state class ke andar rehti hai jo next state object return karti hai.
