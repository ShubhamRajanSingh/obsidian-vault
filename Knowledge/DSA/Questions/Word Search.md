---
tags:
  - dsa
  - backtracking
  - matrix
  - medium
  - leetcode
  - striver
aliases:
  - Word Search
  - LeetCode 79
difficulty: Medium
topic: Backtracking / DFS / Matrix
pattern: DFS backtracking on grid
leetcode_id: 79
date: 2026-05-19
---

# Word Search

## 📌 Problem Statement

> Given an m×n grid of characters and a string `word`, return `true` if `word` exists in the grid. The word can be constructed from sequentially adjacent cells (horizontally or vertically), and each cell may only be used once.

**LeetCode #79** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        for (int i = 0; i < board.length; i++)
            for (int j = 0; j < board[0].length; j++)
                if (dfs(board, word, i, j, 0)) return true;
        return false;
    }

    private boolean dfs(char[][] board, String word, int i, int j, int k) {
        if (k == word.length()) return true;
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] != word.charAt(k)) return false;

        char tmp = board[i][j];
        board[i][j] = '#';
        boolean found = dfs(board, word, i+1, j, k+1) || dfs(board, word, i-1, j, k+1)
                     || dfs(board, word, i, j+1, k+1) || dfs(board, word, i, j-1, k+1);
        board[i][j] = tmp;
        return found;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m × n × 4^L) where L = word length |
| Space | O(L) |
