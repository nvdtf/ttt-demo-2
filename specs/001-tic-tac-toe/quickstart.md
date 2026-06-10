# Quickstart Validation Guide: Tic-Tac-Toe Game

**Feature**: 001-tic-tac-toe | **Date**: 2026-06-10

## Prerequisites

- A modern web browser (Chrome, Firefox, Safari, or Edge)
- No server, build tool, or runtime required

## Setup

1. Ensure `index.html` exists at the repository root.
2. Open `index.html` directly in a web browser (double-click or `File > Open`).

That's it. No install, no build, no server (SC-004).

## Validation Scenarios

### Scenario 1: Play a Complete Game — X Wins (User Story 1)

**Verifies**: FR-001, FR-002, FR-003, FR-004, FR-006, FR-008, FR-009

1. Open `index.html` in a browser.
2. Confirm the status reads **"Player X's turn"** and a 3x3 empty grid is displayed.
3. Click the top-left cell → **X** appears, status changes to **"Player O's turn"**.
4. Click the center cell → **O** appears, status changes to **"Player X's turn"**.
5. Click the top-center cell → **X** appears.
6. Click the bottom-left cell → **O** appears.
7. Click the top-right cell → **X** appears.
8. **Expected result**: Status reads **"Player X wins!"**, the top row (cells 0, 1, 2) is visually highlighted, and clicking any cell has no effect.

### Scenario 2: Draw Game (User Story 1)

**Verifies**: FR-005, FR-006

1. Start a new game (click "New Game" or reload).
2. Play moves in this order (cell indices): X→0, O→1, X→2, O→4, X→3, O→6, X→7, O→8, X→5.
3. **Expected result**: All cells filled, status reads **"It's a draw!"**, no cells highlighted.

### Scenario 3: New Game Reset (User Story 2)

**Verifies**: FR-007, SC-002

1. Complete a game (win or draw).
2. Click **"New Game"**.
3. **Expected result**: Board clears to empty, status resets to **"Player X's turn"**, game is playable again. Reset completes within 1 second.

### Scenario 4: Mid-Game Reset (User Story 2)

**Verifies**: FR-007

1. Make 2-3 moves.
2. Click **"New Game"** before the game ends.
3. **Expected result**: Board clears immediately with no confirmation prompt. Fresh game starts.

### Scenario 5: Occupied Cell Rejection (User Story 3, Edge Case)

**Verifies**: FR-003

1. Click a cell to place X.
2. Click the same cell again.
3. **Expected result**: Nothing happens. Turn does not change. The mark remains X.

### Scenario 6: Responsive Layout (Edge Case)

**Verifies**: FR-012, SC-005

1. Open `index.html` in a browser.
2. Resize the browser window to 320px wide.
3. **Expected result**: The board remains usable, centered, and fully visible. No horizontal scrolling required.

### Scenario 7: Column Win (User Story 1)

**Verifies**: FR-004

1. Start a new game.
2. Play: X→0, O→1, X→3, O→4, X→6.
3. **Expected result**: X wins via left column. Status reads **"Player X wins!"**, cells 0, 3, 6 highlighted.

### Scenario 8: Diagonal Win (User Story 1)

**Verifies**: FR-004

1. Start a new game.
2. Play: X→0, O→1, X→4, O→2, X→8.
3. **Expected result**: X wins via diagonal. Status reads **"Player X wins!"**, cells 0, 4, 8 highlighted.

## All Win Conditions (SC-003)

For completeness, these 8 winning lines must all be detected:

| Line | Cells | Type |
|------|-------|------|
| Top row | 0, 1, 2 | Row |
| Middle row | 3, 4, 5 | Row |
| Bottom row | 6, 7, 8 | Row |
| Left column | 0, 3, 6 | Column |
| Middle column | 1, 4, 7 | Column |
| Right column | 2, 5, 8 | Column |
| Main diagonal | 0, 4, 8 | Diagonal |
| Anti-diagonal | 2, 4, 6 | Diagonal |

See `data-model.md` for the cell index layout and `contracts/ui-contract.md` for expected UI behaviors.
