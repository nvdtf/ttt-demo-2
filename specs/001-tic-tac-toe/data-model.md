# Data Model: Tic-Tac-Toe Game

**Feature**: 001-tic-tac-toe | **Date**: 2026-06-10

## Entities

### Board

Represents the 3x3 playing grid.

| Field | Type | Description |
|-------|------|-------------|
| `cells` | `Array<string>` (length 9) | Flat array indexed 0-8. Each element is `''` (empty), `'X'`, or `'O'`. Index layout: `[0,1,2,3,4,5,6,7,8]` maps to row-major order (top-left to bottom-right). |

**Validation rules**:
- Array length is always exactly 9
- Each element is one of: `''`, `'X'`, `'O'`
- The count of X marks is equal to or exactly one more than the count of O marks (X always goes first)

### Game State

Represents the current status of the game.

| Field | Type | Description |
|-------|------|-------------|
| `board` | `Array<string>` | The board cells as defined above |
| `currentPlayer` | `'X'` \| `'O'` | Whose turn it is. Starts as `'X'` (FR-002) |
| `gameOver` | `boolean` | Whether the game has ended (win or draw) |
| `winner` | `'X'` \| `'O'` \| `null` | The winning player, or `null` if no winner yet |
| `winningCells` | `Array<number>` \| `null` | Indices of the three cells forming the winning line, or `null` |

**State transitions**:

```
INITIAL STATE
  board: ['','','','','','','','','']
  currentPlayer: 'X'
  gameOver: false
  winner: null
  winningCells: null

  ──► MOVE (click empty cell while !gameOver)
      1. Place currentPlayer mark in clicked cell
      2. Check for win → if win: set gameOver=true, winner=currentPlayer, winningCells=[indices]
      3. Check for draw (all cells filled, no winner) → if draw: set gameOver=true
      4. If !gameOver: toggle currentPlayer ('X'↔'O')

  ──► RESET (click "New Game" at any time)
      Return to INITIAL STATE
```

### Winning Combinations

Fixed constant — the 8 possible winning lines as index triples.

```
WINNING_COMBOS = [
  [0, 1, 2],  // top row
  [3, 4, 5],  // middle row
  [6, 7, 8],  // bottom row
  [0, 3, 6],  // left column
  [1, 4, 7],  // middle column
  [2, 5, 8],  // right column
  [0, 4, 8],  // diagonal top-left to bottom-right
  [2, 4, 6],  // diagonal top-right to bottom-left
]
```

### Cell Index Layout

```
 0 | 1 | 2
-----------
 3 | 4 | 5
-----------
 6 | 7 | 8
```

## Relationships

- **Game State** contains one **Board** (composition)
- **Game State** references **Winning Combinations** to detect wins
- There is no Player entity beyond the `'X'`/`'O'` string markers — players are implicit (two humans sharing the same screen)
