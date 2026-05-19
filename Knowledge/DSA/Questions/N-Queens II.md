---
tags:
  - dsa
  - backtracking
  - hard
  - leetcode
  - striver
aliases:
  - N-Queens II
  - LeetCode 52
difficulty: Hard
topic: Backtracking
pattern: Backtracking with constraint checking
leetcode_id: 52
date: 2026-05-19
---

# N-Queens II

## 📌 Problem Statement

> The n-queens puzzle. Return the number of distinct solutions.

**LeetCode #52** | Difficulty: 🔴 Hard

---

## ✅ Java Solution

```java
class Solution {
    int count = 0;

    public int totalNQueens(int n) {
        backtrack(new boolean[n], new boolean[2 * n], new boolean[2 * n], 0, n);
        return count;
    }

    private void backtrack(boolean[] cols, boolean[] diag1, boolean[] diag2, int row, int n) {
        if (row == n) { count++; return; }
        for (int col = 0; col < n; col++) {
            if (cols[col] || diag1[row - col + n] || diag2[row + col]) continue;
            cols[col] = diag1[row - col + n] = diag2[row + col] = true;
            backtrack(cols, diag1, diag2, row + 1, n);
            cols[col] = diag1[row - col + n] = diag2[row + col] = false;
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n!) |
| Space | O(n) |
