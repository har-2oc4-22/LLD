# Lecture 39: Memento Design Pattern (Snapshots & Rollbacks)

---

## 🎯 What is Memento Pattern?

> **Definition**: *"Without violating encapsulation, capture and externalize an object's internal state so that the object can be restored to this state later."*

Memento pattern kisi object ke internal state ka **Snapshot (Backup)** lene aur zaroorat padne par us snapshot par wapas **Rollback (Undo)** karne ke liye use hota hai, bina object ke private data encapsulation ko break kiye.

---

## 💡 Real-World Analogy: Video Game Checkpoints 🎮 & DB Transactions 💾

1. **Game Save State**:
   - Boss fight se pehle game ek **Checkpoint (Memento)** create karta hai.
   - Agar aapka character fight mein die ho gaya, toh game reload karke aapko wapas usi checkpoint par restore kar deta hai.
2. **Database Transactions**:
   - `BEGIN TRANSACTION`: Ek backup snapshot create hota hai.
   - Agar saari queries pass ho gayi: `COMMIT` (snapshot delete).
   - Agar beech me koi query fail ho gayi: `ROLLBACK` (wapas snapshot state restore).

---

## 🏗️ The 3 Core Roles in Memento

```text
[Originator] (Database)
    │ creates & restores
    ▼
[Memento] (DatabaseMemento: Immutable Snapshot)
    ▲
    │ stores without modifying
[Caretaker] (TransactionManager / Undo History)
```

1. **Originator**: The object whose state matters. Ye snapshot banata hai (`createMemento()`) aur restore karta hai (`restoreFromMemento()`).
2. **Memento**: Pure state container (immutable). Iske paas koi business logic nahi hoti, sirf saved data hota hai.
3. **Caretaker**: Snapshots ki list/stack maintain karta hai. Caretaker kabhi bhi memento ke andar ka data inspect ya change nahi karta, sirf hold karta hai.

---

## 💻 Code Walkthrough

```java
// 1. Memento: Immutable Snapshot
class DatabaseMemento {
    private final Map<String, String> data;

    public DatabaseMemento(Map<String, String> dbData) {
        this.data = new HashMap<>(dbData); // Deep copy of state!
    }

    public Map<String, String> getState() { return data; }
}

// 2. Originator: Database
class Database {
    private Map<String, String> records = new HashMap<>();

    public void insert(String key, String value) { records.put(key, value); }

    public DatabaseMemento createMemento() {
        return new DatabaseMemento(records); // Snapshot
    }

    public void restoreFromMemento(DatabaseMemento memento) {
        this.records = new HashMap<>(memento.getState()); // Rollback
    }
}

// 3. Caretaker: TransactionManager
class TransactionManager {
    private DatabaseMemento backup;

    public void beginTransaction(Database db) {
        backup = db.createMemento(); // Takes backup
    }

    public void rollbackTransaction(Database db) {
        if (backup != null) {
            db.restoreFromMemento(backup); // Restores backup
            backup = null;
        }
    }

    public void commitTransaction() {
        backup = null; // Commits and clears backup
    }
}
```

---

## 🌟 Why Encapsulation is Preserved

Traditional approach mein Caretaker ko Originator ke saare private variables get karke bahar store karne padte, jisse encapsulation violate ho jaati.
Memento pattern mein Originator khud apna snapshot object banata hai aur restore karta hai — bahar kisi ko private fields ki direct knowledge nahi hoti.

---

## 📝 Lecture 39 Summary (Quick Revision)

- **Undo / Redo / Rollback**: The go-to pattern for transactional integrity and rollback history.
- **Three Roles**: Originator (owns state), Memento (stores state snapshot), Caretaker (manages history stack).
- **Deep Copy Requirement**: Memento banate waqt mutable collections (Lists, Maps) ka shallow reference nahi, balki fresh copy banana zaroori hai.
