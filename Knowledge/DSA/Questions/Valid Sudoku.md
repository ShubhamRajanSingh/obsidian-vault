---
tags:
  - dsa
  - array
  - hashset
  - medium
  - leetcode
aliases:
  - Valid Sudoku
  - LeetCode 36
difficulty: Medium
topic: Array / HashSet
pattern: Row/Col/Box validation
leetcode_id: 36
date: 2026-05-19
---

# Valid Sudoku

## 📌 Problem Statement

> Determine if a 9×9 Sudoku board is valid. Only filled cells need to be validated (rows, columns, and 3×3 boxes must not have duplicates among 1-9).

**LeetCode #36** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        Set<String> seen = new HashSet<>();

        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char c = board[i][j];
                if (c == '.') continue;
                if (!seen.add(c + " row" + i) ||
                    !seen.add(c + " col" + j) ||
                    !seen.add(c + " box" + (i/3) + (j/3))) return false;
            }
        }
        return true;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(81) = O(1) |
| Space | O(81) = O(1) |
