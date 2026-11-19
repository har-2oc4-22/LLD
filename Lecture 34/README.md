# Lecture 34: Machine Coding Case Study - Snake & Ladder Board Game

---

## 🎯 System Overview

Is case study mein classic **Snake & Ladder Board Game** ka complete Low-Level Design cover kiya gaya hai.
System handle karta hai:
- Customizable Board dimensions (e.g. Standard 100 cells, or custom size).
- Dynamic Board Entities (`Snake` with head > tail, `Ladder` with bottom < top).
- Multi-Dice rolling mechanics with configurable dice count.
- Pluggable Board Generation Strategies (Standard board vs Random board generator).
- Exact 100 win-condition rules.
- Queue-based multiplayer turn management.

---

## 📋 Functional Requirements

1. **Board Configuration**:
   - Grid size from cell 1 to cell $N$ (default 100).
   - Snakes: Land hone par player drop ho kar tail par chala jata hai.
   - Ladders: Land hone par player climb karke top par chala jata hai.
   - No infinite loop guarantee (Snake head cannot be ladder foot).
2. **Dice Rolling**:
   - Generates random integer between $1 \times \text{diceCount}$ to $6 \times \text{diceCount}$.
3. **Movement Rules**:
   - Player position cannot exceed 100. Agar current pos 97 hai aur dice 5 aaye, toh move discard ho jata hai (player remains on 97).
4. **Game Loop**:
   - Round-robin turn rotation using `Deque`.

---

## 🏗️ Design Patterns Applied

| Pattern | Component | Why Chosen |
| :--- | :--- | :--- |
| **Strategy Pattern** | `BoardSetupStrategy` (`RandomBoardSetupStrategy`, `StandardBoardSetupStrategy`) | Board generation algorithms ko decouple karta hai (e.g. static tournament board vs random casual board). |
| **Strategy Pattern** | `SnakeAndLadderRules` | Win conditions aur movement bounds ko encapsulate karta hai. |
| **Observer Pattern** | `IObserver`, `SnakeAndLadderConsoleNotifier` | Move updates, snake bites, ladder climbs, aur win announcements ko console/UI par broadcast karta hai. |
| **Factory Pattern** | `SnakeAndLadderGameFactory` | Complete game assembly and configuration. |

---

## 💻 Code Walkthrough

```java
// 1. Board Entity Hierarchy (Inheritance)
abstract class BoardEntity {
    protected int start, end;
    public int getEnd() { return end; }
}

class Snake extends BoardEntity {
    public Snake(int head, int tail) {
        if (head <= tail) throw new IllegalArgumentException("Snake head must be > tail");
        this.start = head; this.end = tail;
    }
}

class Ladder extends BoardEntity {
    public Ladder(int bottom, int top) {
        if (bottom >= top) throw new IllegalArgumentException("Ladder bottom must be < top");
        this.start = bottom; this.end = top;
    }
}

// 2. Board Setup Strategy
interface BoardSetupStrategy {
    void setupBoard(Board board, int numSnakes, int numLadders);
}

// 3. Game Loop Movement
int newPos = currentPos + diceRoll;
if (newPos <= board.getSize()) {
    // Check if snake or ladder exists at newPos
    if (board.hasEntityAt(newPos)) {
        newPos = board.getEntityAt(newPos).getEnd();
    }
    player.setPosition(newPos);
}
```

---

## 📝 Lecture 34 Summary (Quick Revision)

- **Abstraction for Board Entities**: `Snake` aur `Ladder` dono `BoardEntity` (start, end) inherit karte hain, code duplication nahi hoti.
- **Pluggable Setup**: Strategy pattern se tournament matches ke fixed boards aur casual matches ke randomized boards easily switch ho jaate hain.
- **Round-Robin Turn Handling**: `Deque` guarantees fair turn scheduling.
