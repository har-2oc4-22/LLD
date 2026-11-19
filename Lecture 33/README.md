# Lecture 33: Machine Coding Case Study - Tic-Tac-Toe Game ($N \times N$ Grid)

---

## 🎯 System Overview

Is case study mein generic **$N \times N$ Tic-Tac-Toe Game** ka clean Low-Level Design cover kiya gaya hai.
Yeh problem software engineering interviews (especially Amazon, Microsoft, Uber) ke machine coding rounds mein frequently pucha jaata hai.

---

## 📋 Functional Requirements

1. **Customizable Grid Size**: $3 \times 3$, $4 \times 4$, ya generic $N \times N$ board support.
2. **Multiplayer Turn Management**: Multiple players with distinct symbols ('X', 'O', '#') taking turns in round-robin fashion.
3. **Move Validation**: Bound checking aur already occupied cell protection.
4. **Win & Draw Detection**:
   - Win condition: Any row, column, main diagonal, or anti-diagonal filled completely with the same symbol.
   - Draw condition: All cells filled without any winner.
5. **Real-time Game Event Notifications**: Observer pattern notifying players/spectators of each move.

---

## 🔄 Turn Management: The Queue Trick

Turn management ke liye sabse clean aur elegant data structure **`Deque<TicTacToePlayer>`** (Double-ended Queue) hota hai:

```text
Turn Start:
[Player 1] -> [Player 2]
    │
    ▼ (P1 makes move)
Valid Move?
    ├── YES: P1 popped from front, pushed to back -> [Player 2] -> [Player 1]
    └── NO: P1 stays at front, asks to retry!
```

---

## 🧮 Win Condition Evaluation ($O(N)$ Time)

Har move par poora board scan karne ki zaroorat nahi hoti:
1. **Row Check**: Check if all cells in current `row` match symbol.
2. **Column Check**: Check if all cells in current `col` match symbol.
3. **Main Diagonal**: If `row == col`, check main diagonal ($i == j$).
4. **Anti-Diagonal**: If `row + col == size - 1`, check anti-diagonal.

---

## 🏗️ Design Patterns Applied

| Pattern | Component | Why Chosen |
| :--- | :--- | :--- |
| **Strategy Pattern** | `TicTacToeRules` | Win rules, grid constraints aur boundary logic ko game loop se decouple karta hai. |
| **Observer Pattern** | `IObserver`, `ConsoleNotifier` | Game events (move made, winner declared, draw) ko UI/Console se decouple karta hai. |
| **Factory Pattern** | `TicTacToeGameFactory` | Game configurations instantiate karta hai. |

---

## 💻 Code Walkthrough

```java
// 1. Dumb Board Object (Only manages grid array)
class Board {
    private Symbol[][] grid;
    private int size;

    public boolean placeMark(int row, int col, Symbol mark) {
        if (!isCellEmpty(row, col)) return false;
        grid[row][col] = mark;
        return true;
    }
}

// 2. Rules Strategy
interface TicTacToeRules {
    boolean isValidMove(Board b, int r, int c);
    boolean checkWinCondition(Board b, Symbol s);
    boolean checkDrawCondition(Board b);
}

// 3. Game Orchestrator Loop
class TicTacToeGame {
    private Deque<TicTacToePlayer> players = new ArrayDeque<>();
    private Board board;

    public void playTurn(int r, int c) {
        TicTacToePlayer current = players.peekFirst();
        if (rules.isValidMove(board, r, c)) {
            board.placeMark(r, c, current.getSymbol());
            if (rules.checkWinCondition(board, current.getSymbol())) {
                System.out.println(current.getName() + " wins!");
                return;
            }
            // Rotate turn
            players.removeFirst();
            players.addLast(current);
        }
    }
}
```

---

## 📝 Lecture 33 Summary (Quick Revision)

- **Separation of Board vs Rules**: Board class sirf data store karti hai; rules class game logic check karti hai (SRP).
- **Deque for Turns**: Avoids error-prone index tracking (`(turn + 1) % numPlayers`).
- **Scalable to $N \times N$**: Code is written generically without hardcoded `3` constants.
