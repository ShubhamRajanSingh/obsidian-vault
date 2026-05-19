---
tags:
  - dsa
  - dynamic-programming
  - medium
  - leetcode
  - striver
aliases:
  - Minimum Path Sum
  - LeetCode 64
difficulty: Medium
topic: Dynamic Programming / Matrix
pattern: 2D DP
leetcode_id: 64
date: 2026-05-19
---

# Minimum Path Sum

## 📌 Problem Statement

> Given an m×n grid of non-negative integers, find a path from top-left to bottom-right that minimizes the sum of numbers along its path (only move right or down).

**LeetCode #64** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) continue;
                else if (i == 0) grid[i][j] += grid[i][j - 1];
                else if (j == 0) grid[i][j] += grid[i - 1][j];
                else grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
            }
        return grid[m - 1][n - 1];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m × n) |
| Space | O(1) in-place |
