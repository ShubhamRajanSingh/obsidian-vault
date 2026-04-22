# Sudoku Solver — DSA Deep Dive

## Problem Statement

Given a 9×9 partially filled Sudoku board, fill in the empty cells (represented by `'.'`) such that:

- Each **row** contains digits `1–9` with no repetition.
- Each **column** contains digits `1–9` with no repetition.
- Each of the nine **3×3 sub-boxes** contains digits `1–9` with no repetition.

> It is guaranteed that the input board has exactly one solution.

**LeetCode:** [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

---

## Approach — Backtracking

### Core Idea

Sudoku Solver is a classic **Constraint Satisfaction Problem (CSP)** solved using **Backtracking** — a refined form of brute force that abandons a path as soon as a constraint is violated.

### Algorithm Steps

```
1. Scan the board left-to-right, top-to-bottom.
2. Find the next empty cell ('.').
3. Try placing digits '1' through '9' in that cell.
4. For each digit, check if it is VALID (not already in same row, column, or 3×3 box).
5. If valid → place it and recurse to the next empty cell.
6. If recursion returns true → puzzle solved, propagate true upward.
7. If recursion returns false → BACKTRACK (reset cell to '.') and try next digit.
8. If no digit works → return false (triggers backtracking in caller).
```

### Why Backtracking Works Here

- The **search space** is finite: at most 9 choices per cell, at most 81 cells.
- Constraints eliminate most branches early — in practice the tree is very shallow.
- Guaranteed unique solution means one branch always leads to success.

---

## Complexity Analysis

| Metric | Value | Notes |
|--------|-------|-------|
| Time   | O(9^m) | m = number of empty cells; pruning cuts this drastically in practice |
| Space  | O(m)   | Recursion stack depth = number of empty cells |

In the worst case there are 81 empty cells, giving O(9^81), but constraint checks prune the tree so aggressively that real-world performance is near-instant for valid boards.

---

## Validity Check — How It Works

Before placing a digit `d` at `(row, col)`:

```
Row check    → scan board[row][0..8], d must not appear
Column check → scan board[0..8][col], d must not appear
Box check    → compute box's top-left corner:
                   boxRow = (row / 3) * 3
                   boxCol = (col / 3) * 3
               scan the 3×3 sub-grid, d must not appear
```

---

## Java Code

```java
class Solution {

    public void solveSudoku(char[][] board) {
        solve(board);
    }

    // Returns true if board is completely and correctly filled
    private boolean solve(char[][] board) {
        for (int row = 0; row < 9; row++) {
            for (int col = 0; col < 9; col++) {

                // Found an empty cell
                if (board[row][col] == '.') {

                    // Try every digit 1–9
                    for (char d = '1'; d <= '9'; d++) {

                        if (isValid(board, row, col, d)) {
                            board[row][col] = d;          // Place digit

                            if (solve(board)) {
                                return true;              // Puzzle solved
                            }

                            board[row][col] = '.';        // Backtrack
                        }
                    }

                    // No digit worked → trigger backtrack in caller
                    return false;
                }
            }
        }

        // No empty cell found → board is fully filled
        return true;
    }

    // Check if placing digit d at (row, col) is legal
    private boolean isValid(char[][] board, int row, int col, char d) {

        // Row check
        for (int c = 0; c < 9; c++) {
            if (board[row][c] == d) return false;
        }

        // Column check
        for (int r = 0; r < 9; r++) {
            if (board[r][col] == d) return false;
        }

        // 3×3 box check
        int boxRow = (row / 3) * 3;
        int boxCol = (col / 3) * 3;

        for (int r = boxRow; r < boxRow + 3; r++) {
            for (int c = boxCol; c < boxCol + 3; c++) {
                if (board[r][c] == d) return false;
            }
        }

        return true;
    }
}
```

---

## Step-by-Step Dry Run

Consider this board segment (first few cells):

```
5 3 . | . 7 . | . . .
6 . . | 1 9 5 | . . .
. 9 8 | . . . | . 6 .
```

**Execution trace for cell (0,2) — first empty cell:**

```
Try '1' → valid? Row has 5,3 ✓ | Col 2 has 8 ✓ | Box has 5,3,6,9,8 ✓ → place '1'
  Recurse → next empty cell (0,3)
    Try '1' → Col 3 already has '1' at (1,3) → skip
    Try '2' → valid → place '2'
      Recurse...
      ... eventually contradiction found deeper
    Backtrack → (0,3) = '.'
    Try '3' → Row 0 already has '3' → skip
    ...
    Try '6' → valid → place '6'
      Recurse → eventually works out
  Return true
Return true
```

---

## Optimized Approach — Bitmask (Advanced)

Instead of scanning rows/columns/boxes each time, maintain **three boolean arrays** (or bitmasks) updated in O(1):

```java
boolean[] rowUsed   = new boolean[9];   // rowUsed[r][d] → digit d used in row r
boolean[] colUsed   = new boolean[9];   // colUsed[c][d] → digit d used in col c
boolean[] boxUsed   = new boolean[9];   // boxUsed[b][d] → digit d used in box b
```

Box index: `b = (row / 3) * 3 + (col / 3)`

This reduces each validity check from **O(27)** to **O(1)**, giving a significant constant-factor speedup.

```java
class SolutionOptimized {

    boolean[][] rowUsed = new boolean[9][10];
    boolean[][] colUsed = new boolean[9][10];
    boolean[][] boxUsed = new boolean[9][10];

    public void solveSudoku(char[][] board) {

        // Pre-fill from existing clues
        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                if (board[r][c] != '.') {
                    int d = board[r][c] - '0';
                    int b = (r / 3) * 3 + (c / 3);
                    rowUsed[r][d] = colUsed[c][d] = boxUsed[b][d] = true;
                }
            }
        }

        solve(board, 0, 0);
    }

    private boolean solve(char[][] board, int row, int col) {
        if (row == 9) return true;                      // All rows done
        if (col == 9) return solve(board, row + 1, 0);  // Move to next row

        if (board[row][col] != '.') {
            return solve(board, row, col + 1);           // Skip filled cells
        }

        int b = (row / 3) * 3 + (col / 3);

        for (int d = 1; d <= 9; d++) {
            if (!rowUsed[row][d] && !colUsed[col][d] && !boxUsed[b][d]) {
                board[row][col] = (char) ('0' + d);
                rowUsed[row][d] = colUsed[col][d] = boxUsed[b][d] = true;

                if (solve(board, row, col + 1)) return true;

                board[row][col] = '.';
                rowUsed[row][d] = colUsed[col][d] = boxUsed[b][d] = false;
            }
        }

        return false;
    }
}
```

---

## Key Concepts Used

| Concept | Role in This Problem |
|---------|----------------------|
| **Backtracking** | Core algorithm — try, recurse, undo on failure |
| **Constraint Propagation** | `isValid()` eliminates illegal placements early |
| **Recursion** | Natural fit for fill-one-cell-then-recurse pattern |
| **Bitmask / Boolean Array** | O(1) validity check in optimized version |
| **DFS on Implicit Tree** | Each board state is a node; digits tried = branches |

---

## Common Mistakes to Avoid

1. **Forgetting to backtrack** — always reset `board[row][col] = '.'` after failed recursion.
2. **Off-by-one in box index** — use `(row/3)*3` not `row/3` for top-left corner.
3. **Checking the placed cell itself** — the validity check scans the whole row/col/box but the cell is still `'.'` when `isValid` is called, so no false self-conflict.
4. **Not returning `false` after exhausting all digits** — this is what triggers the caller to backtrack.

---

## Related Problems

| Problem | Type |
|---------|------|
| [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/) | Validation only (no solving) |
| [N-Queens](https://leetcode.com/problems/n-queens/) | Backtracking, placement constraints |
| [Word Search](https://leetcode.com/problems/word-search/) | Backtracking on a 2D grid |
| [Rat in a Maze](https://www.geeksforgeeks.org/rat-in-a-maze/) | Classic backtracking |

---

*Difficulty: Hard | Tags: Backtracking, Array, Matrix, Constraint Satisfaction*
