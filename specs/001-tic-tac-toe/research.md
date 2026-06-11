# Research: Tic-Tac-Toe Game

**Feature**: 001-tic-tac-toe | **Date**: 2026-06-10

## Overview

The technical context for this feature contains no NEEDS CLARIFICATION items. The constitution (Principle IV) and spec (FR-010, FR-011) fully prescribe the technology constraints. Research focuses on best practices for the chosen approach.

## Research Findings

### 1. Win Detection Algorithm

**Decision**: Array of winning combinations checked after each move.

**Rationale**: A 3x3 board has exactly 8 winning lines (3 rows, 3 columns, 2 diagonals). Enumerating them as an array of index triples and checking after each move is O(1) in practice (8 fixed checks), trivially correct, and easy to read. No need for more sophisticated algorithms at this scale.

**Alternatives considered**:
- Bitboard representation — overkill for 3x3, adds complexity with no benefit
- Magic square mapping — clever but obscure, harder to maintain
- Row/column/diagonal counters — slightly more code, marginal performance gain irrelevant at this scale

### 2. Board State Representation

**Decision**: Flat array of 9 elements (indices 0-8), each holding `''`, `'X'`, or `'O'`.

**Rationale**: A flat array maps directly to cell indices, simplifies win-condition checks (winning combos are index triples), and is the most straightforward representation. A 2D array would require row/col indexing that adds complexity without benefit.

**Alternatives considered**:
- 2D array (3x3) — requires `board[row][col]` access; win checks need nested indexing
- Object with string keys — no advantage over array for fixed-size grid

### 3. Responsive Board Sizing (vmin-based)

**Decision**: Use `vmin` units for board dimensions, capped with `max-width` and `max-height` in pixels.

**Rationale**: FR-012 specifies vmin-based sizing capped at a max pixel size. `vmin` ensures the board scales with the smaller viewport dimension, keeping it square and visible on both portrait and landscape orientations. A pixel cap prevents the board from becoming unwieldy on large screens. CSS Grid or simple table/div structure for the 3x3 layout.

**Alternatives considered**:
- Percentage-based sizing — doesn't maintain square aspect ratio as cleanly
- Fixed pixel sizes with media queries — more CSS, less fluid

### 4. Single-File Architecture

**Decision**: All HTML, CSS (`<style>`), and JS (`<script>`) inline in one `index.html`.

**Rationale**: FR-011 mandates a single file. Inline `<style>` and `<script>` blocks are the standard approach. CSS custom properties (variables) can keep the style section organized. The JS module pattern (IIFE or event-listener-based initialization) avoids polluting the global namespace.

**Alternatives considered**:
- Separate files linked via relative paths — explicitly prohibited by FR-011
- Data URIs for assets — not needed; no external assets required

### 5. Deferred-to-Probe Decisions

**Decision**: Make reasonable default choices for D2, D3, D5, D6, D7 in the probe; these will be evaluated and potentially revised after deployment.

**Rationale**: The spec explicitly defers these dimensions to the probe phase (per Constitution Principle III — Deployable Probe First). The probe should ship with sensible defaults that can be easily changed.

**Reasonable defaults for the probe**:
- D2 (Mark Rendering): Plain text characters styled with CSS — simplest, meets spec
- D3 (Win Highlight): Background color change on winning cells — straightforward CSS
- D5 (Cell Hover): Subtle background highlight on empty cells — low-effort UX improvement
- D6 (Color Palette): Neutral base with one accent color — clean, accessible
- D7 (Status Placement): Above the board (status, board, button top-to-bottom) — conventional layout

**Alternatives considered**: All options listed in spec; will be evaluated post-probe.
