# UI Contract: Tic-Tac-Toe Game

**Feature**: 001-tic-tac-toe | **Date**: 2026-06-10

## Interface Overview

The game exposes a single-page UI consisting of three elements: a status display, a 3x3 interactive grid, and a reset button. All interaction is via mouse clicks on the grid cells and the button.

## UI Elements

### Status Display

| Property | Contract |
|----------|----------|
| Location | Above the board (D7 probe default) |
| Content | Text string reflecting current game state |
| States | `"Player X's turn"`, `"Player O's turn"`, `"Player X wins!"`, `"Player O wins!"`, `"It's a draw!"` |
| Updates | Immediately after each move or game reset |

### Game Board (3x3 Grid)

| Property | Contract |
|----------|----------|
| Layout | 3 rows x 3 columns, square cells |
| Sizing | vmin-based dimensions, capped at a maximum pixel size (FR-012) |
| Minimum usable width | 320px viewport (SC-005) |
| Cell content | Empty, `"X"`, or `"O"` (plain text, D2 probe default) |

### New Game Button

| Property | Contract |
|----------|----------|
| Label | `"New Game"` |
| Location | Below the board |
| Availability | Always visible and clickable |

## Interaction Contract

### Cell Click

| Condition | Behavior |
|-----------|----------|
| Cell is empty AND game is not over | Place current player's mark, check for win/draw, toggle turn |
| Cell is occupied | No effect (FR-003) |
| Game is over | No effect (FR-008) |

### New Game Click

| Condition | Behavior |
|-----------|----------|
| Any time (during or after game) | Reset board to empty, set turn to Player X, clear winner/highlight |
| Response time | < 1 second (SC-002) |

### Hover (probe default)

| Condition | Behavior |
|-----------|----------|
| Empty cell, game not over | Subtle background highlight (D5 probe default) |
| Occupied cell or game over | No hover effect |

## Visual Feedback Contract

### Win Highlight

| Trigger | Behavior |
|---------|----------|
| A player completes a winning line | The three winning cells receive a distinct background color (D3 probe default) |
| Duration | Persists until "New Game" is clicked |

### Turn Indicator

| Trigger | Behavior |
|---------|----------|
| After each valid move | Status text updates to show next player's turn |
| After game ends | Status text shows winner or draw result |

## Accessibility Notes

- Cells should be keyboard-reachable (semantic HTML buttons or equivalent)
- Status updates should be perceivable (visible text, not solely color-dependent)
- Sufficient color contrast for marks and status text
