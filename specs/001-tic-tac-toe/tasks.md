# Tasks: Tic-Tac-Toe Game

**Input**: Design documents from `/specs/001-tic-tac-toe/`

**Prerequisites**: plan.md, spec.md, data-model.md, contracts/ui-contract.md, research.md, quickstart.md

**Tests**: Not requested in the feature specification. No test tasks are included.

**Organization**: Tasks are grouped by user story. All tasks modify a single file (`index.html`) per FR-011, so no tasks are marked [P] (parallel requires different files).

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies) — not applicable for this single-file project
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- All file paths reference `index.html` at the repository root

---

## Phase 1: Setup

**Purpose**: Create the single HTML file with boilerplate structure

- [ ] T001 Create `index.html` at the repository root with HTML5 doctype, `<html>`, `<head>` (with charset, viewport meta, title "Tic-Tac-Toe"), `<body>` containing a container `<div>`, an empty `<style>` block in `<head>`, and an empty `<script>` block before `</body>`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core game state, constants, initialization, rendering, and base CSS that ALL user stories depend on

**CRITICAL**: No user story work can begin until this phase is complete

- [ ] T002 Define the `WINNING_COMBOS` constant (array of 8 index-triples per data-model.md) and game state variables (`board` as a 9-element array, `currentPlayer` as `'X'`/`'O'`, `gameOver` as boolean, `winner` as `'X'`/`'O'`/`null`, `winningCells` as array/null) in the `<script>` block of `index.html`
- [ ] T003 Implement `initGame()` function that resets all state variables to their initial values (empty board, currentPlayer `'X'`, gameOver `false`, winner `null`, winningCells `null`) and calls `renderBoard()`, in the `<script>` block of `index.html`
- [ ] T004 Implement `renderBoard()` function that creates or updates 9 cell elements in a grid container `<div>`, setting each cell's text content from the `board` array, and attach click handlers to each cell, in the `<script>` block of `index.html`
- [ ] T005 Add base CSS in the `<style>` block of `index.html`: centered page layout (flexbox column, centered items), box-sizing border-box, font-family, and 3x3 grid layout for the board container using CSS Grid (`grid-template-columns: repeat(3, 1fr)`), with cell borders, square aspect ratio, cursor pointer, and font sizing for marks
- [ ] T006 Add a `DOMContentLoaded` event listener that calls `initGame()` to bootstrap the game on page load, in the `<script>` block of `index.html`

**Checkpoint**: At this point, opening `index.html` should show a 3x3 grid of empty cells. No game logic yet.

---

## Phase 3: User Story 1 — Play a Complete Game Against Another Human (Priority: P1) MVP

**Goal**: Two players alternate placing X and O marks. The game detects wins (all 8 combinations) and draws. Moves are blocked on occupied cells and after the game ends.

**Independent Test**: Open `index.html`, click cells to place X/O alternately, verify marks appear, win is detected (top row, column, diagonal), draw is detected when all cells fill with no winner, occupied cells reject clicks, and no moves are accepted after game over.

### Implementation for User Story 1

- [ ] T007 [US1] Implement `handleCellClick(index)` function that: (a) ignores clicks if `gameOver` is true (FR-008) or the cell is occupied (FR-003), (b) places `currentPlayer` mark in `board[index]`, (c) calls win/draw check functions, (d) toggles `currentPlayer` between `'X'` and `'O'` if game is not over, (e) calls `renderBoard()`, in the `<script>` block of `index.html`
- [ ] T008 [US1] Implement `checkWin()` function that iterates over `WINNING_COMBOS`, checks if all three cells in any combo match `currentPlayer`, and if so sets `gameOver = true`, `winner = currentPlayer`, and `winningCells` to the matching indices, in the `<script>` block of `index.html`
- [ ] T009 [US1] Implement `checkDraw()` function that checks if all 9 cells are filled and `gameOver` is still false, and if so sets `gameOver = true` (winner remains null), in the `<script>` block of `index.html`
- [ ] T010 [US1] Wire `handleCellClick` into the cell click handlers created by `renderBoard()`, passing the cell index, in the `<script>` block of `index.html`

**Checkpoint**: The game is now fully playable — X and O alternate, wins and draws are detected, occupied/post-game clicks are blocked. This is the deployable probe (Constitution Principle III).

---

## Phase 4: User Story 2 — Start a New Game (Priority: P2)

**Goal**: A "New Game" button resets the board and game state at any time (during or after a game).

**Independent Test**: Play a game to completion, click "New Game", verify the board clears to empty and it becomes Player X's turn. Also test mid-game reset.

### Implementation for User Story 2

- [ ] T011 [US2] Add a `<button>` element with text "New Game" below the board in the HTML of `index.html`, and style it in the `<style>` block (margin, padding, font size, cursor pointer)
- [ ] T012 [US2] Attach a click event listener on the "New Game" button that calls `initGame()` to reset state and re-render the board, in the `<script>` block of `index.html`

**Checkpoint**: Users can restart the game at any point without reloading the page. Reset completes within 1 second (SC-002).

---

## Phase 5: User Story 3 — See Game Status and Visual Feedback (Priority: P3)

**Goal**: A status message shows whose turn it is, who won, or if it's a draw. Winning cells are visually highlighted.

**Independent Test**: Observe the status message updating during gameplay ("Player X's turn" / "Player O's turn"), verify it shows "Player X wins!" or "Player O wins!" on a win with the winning cells highlighted, and shows "It's a draw!" on a draw.

### Implementation for User Story 3

- [ ] T013 [US3] Add a status display element (e.g., `<div id="status">`) above the board in the HTML of `index.html`, and style it in the `<style>` block (font size, margin, text alignment, min-height to prevent layout shift)
- [ ] T014 [US3] Implement `updateStatus()` function that sets the status element text to: "Player X wins!" or "Player O wins!" if there is a winner, "It's a draw!" if gameOver with no winner, or "Player X's turn" / "Player O's turn" during play — call it from `handleCellClick` and `initGame`, in the `<script>` block of `index.html`
- [ ] T015 [US3] Add a CSS class `.winning-cell` with a distinct background color for winning cells in the `<style>` block, and apply it in `renderBoard()` to cells whose indices are in `winningCells` when a win has occurred, in `index.html`

**Checkpoint**: All three user stories are now independently functional. The game shows status, highlights wins, and can be reset.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Responsive sizing, hover effects, accessibility, and final validation

- [ ] T016 Implement vmin-based board sizing (width and height using `vmin` units) capped with `max-width` and `max-height` in pixels, ensuring the board remains usable at 320px viewport width (FR-012, SC-005), in the `<style>` block of `index.html`
- [ ] T017 Add subtle hover background highlight on empty cells when the game is not over (D5 probe default) using a CSS `:hover` selector scoped to non-occupied, non-game-over state, in `index.html`
- [ ] T018 Ensure grid cells are keyboard-accessible by using `<button>` elements (or adding `tabindex` and `keydown` handlers) so users can navigate and activate cells with Tab and Enter, in `index.html`
- [ ] T019 Run all quickstart.md validation scenarios (Scenarios 1-8) by opening `index.html` in a browser and manually verifying each scenario passes

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies — can start immediately
- **Foundational (Phase 2)**: Depends on Setup (T001) — BLOCKS all user stories
- **User Story 1 (Phase 3)**: Depends on Foundational completion (T002-T006)
- **User Story 2 (Phase 4)**: Depends on Foundational completion; logically depends on US1 (needs a game to reset)
- **User Story 3 (Phase 5)**: Depends on Foundational completion; logically depends on US1 (needs game events to display)
- **Polish (Phase 6)**: Depends on all user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Phase 2 — no dependencies on other stories
- **User Story 2 (P2)**: Can start after Phase 2 — uses `initGame()` from Phase 2; independent of US1 but more meaningful after US1
- **User Story 3 (P3)**: Can start after Phase 2 — reads game state set by US1 logic; independent but more meaningful after US1

### Within Each User Story

- All tasks are sequential (single file, each builds on prior changes)
- Core logic before UI wiring
- Story complete before moving to next priority

### Parallel Opportunities

- **Limited**: All tasks modify `index.html`, so true file-level parallelism is not possible
- **Conceptual parallelism**: US2 (T011-T012) and US3 (T013-T015) could be developed concurrently by different developers if they coordinate on the shared file, but sequential execution is recommended for a single-file project

---

## Parallel Example: User Story 1

```text
# All tasks are sequential (same file):
T007 → T008 → T009 → T010

# Within a task, multiple functions can be written together,
# but tasks should be committed individually.
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup (T001)
2. Complete Phase 2: Foundational (T002-T006)
3. Complete Phase 3: User Story 1 (T007-T010)
4. **STOP and VALIDATE**: Open `index.html` in browser, play a complete game — this is the deployable probe (Constitution Principle III)
5. Deploy/demo if ready

### Incremental Delivery

1. Setup + Foundational (T001-T006) → Visible empty grid
2. Add User Story 1 (T007-T010) → Playable game (MVP / probe)
3. Add User Story 2 (T011-T012) → Restartable game
4. Add User Story 3 (T013-T015) → Full status and visual feedback
5. Polish (T016-T019) → Responsive, accessible, validated
6. Each story adds value without breaking previous stories

---

## Notes

- Single-file architecture: all tasks modify `index.html` (FR-011)
- No [P] markers: parallel execution requires different files, which doesn't apply here
- No test tasks: tests were not requested in the specification
- Deferred-to-probe decisions (D2, D3, D5, D6, D7) use reasonable defaults from research.md
- Commit after each task or logical group
- Stop at any checkpoint to validate the story independently
