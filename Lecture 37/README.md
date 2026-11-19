# Lecture 37: Machine Coding Case Study - Chess Game System (Chess.com Core)

---

## 🎯 System Overview

Is case study mein **Chess.com / Lichess** jaise multiplayer online chess platform ka complete Low-Level Design cover kiya gaya hai.
Yeh project OOP concepts, design patterns, aur complex algorithmic rules ka ultimate demonstration hai:
- 6 Piece types (`King`, `Queen`, `Rook`, `Bishop`, `Knight`, `Pawn`) with their unique movement rules.
- Factory Pattern for piece creation.
- Board coordinate validation & obstacle path checking.
- Matchmaking engine with ELO Rating strategy.
- In-game player chat via Mediator Pattern.

---

## 📋 Core Entities & Piece Hierarchy

```text
                           [Piece] (Abstract)
              canMove(Position start, Position end, Board b)
           ┌──────────┬──────────┬──────────┬──────────┬──────────┐
           ▼          ▼          ▼          ▼          ▼          ▼
        [King]     [Queen]     [Rook]    [Bishop]   [Knight]    [Pawn]
        (1 step    (Rook +     (Straight  (Diagonal  (L-shaped  (Forward
         any dir)   Bishop)     lines)     lines)     jump)      1/2 + diag capture)
```

---

## ♟️ Piece Movement Rules (Clean Polymorphism)

1. **Rook**: Same row (`start.row == end.row`) ya same column (`start.col == end.col`) mein travel karta hai; path ke beech koi doosra piece nahi hona chahiye.
2. **Bishop**: Diagonal move (`|start.row - end.row| == |start.col - end.col|`); diagonal path clear hona chahiye.
3. **Queen**: Combination of Rook and Bishop.
4. **Knight (Horse)**: L-shaped movement ($(\Delta r == 1 \land \Delta c == 2) \lor (\Delta r == 2 \land \Delta c == 1)$); board ke baaki pieces ke upar se jump kar sakta hai (no path obstacle check needed).
5. **King**: Kisi bhi direction mein maximum 1 step.
6. **Pawn**: 1 step forward (ya starting position se 2 steps), diagonal capture.

---

## 🏗️ Design Patterns Applied

| Pattern | Component | Why Chosen |
| :--- | :--- | :--- |
| **Factory Pattern** | `PieceFactory` | Board setup ke waqt White aur Black pieces clean way mein instantiate karta hai. |
| **Strategy Pattern** | `ChessRules`, `MatchingStrategy` | Matchmaking (Score/ELO-based vs Rapid) aur chess variants (Standard vs Blitz) ko interchangeable banata hai. |
| **Mediator Pattern** | `ChatMediator`, `Match` | Match ke dauran do opponents ke beech in-game chat messaging handle karta hai. |
| **Singleton Pattern** | `GameManager` | Global matchmaking queue aur active games registry manage karta hai. |

---

## 💻 Code Walkthrough

```java
// 1. Piece Base Contract
abstract class Piece {
    protected Color color;
    protected PieceType type;

    public abstract boolean canMove(Position start, Position end, Board board);
}

// 2. Knight Movement (L-Shape jump)
class Knight extends Piece {
    @Override
    public boolean canMove(Position start, Position end, Board board) {
        int dRow = Math.abs(start.row - end.row);
        int dCol = Math.abs(start.col - end.col);

        // L-shape check
        boolean isLShape = (dRow == 1 && dCol == 2) || (dRow == 2 && dCol == 1);
        if (!isLShape) return false;

        // Destination cannot hold same-color piece
        Piece dest = board.getPiece(end);
        return dest == null || dest.getColor() != this.color;
    }
}

// 3. Move Execution in Match
class Match {
    public boolean makeMove(User player, Position from, Position to) {
        Piece piece = board.getPiece(from);
        if (piece == null || piece.getColor() != currentTurnColor) return false;

        if (piece.canMove(from, to, board)) {
            board.setPiece(to, piece);
            board.setPiece(from, null);
            switchTurn(); // Switch White <-> Black
            return true;
        }
        return false;
    }
}
```

---

## 📝 Lecture 37 Summary (Quick Revision)

- **Polymorphic Move Validation**: Har piece apni legal moves khud janta hai (`canMove()`), eliminating giant `switch(piece.type)` statements.
- **Board Grid**: $8 \times 8$ matrix holding `Piece` references.
- **Match Orchestrator**: Manages clock, player turns, move history, and victory conditions.
