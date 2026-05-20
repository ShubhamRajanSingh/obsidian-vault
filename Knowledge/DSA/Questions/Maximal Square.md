---
tags:
  - dsa
  - dynamic-programming
  - matrix
  - medium
  - leetcode
  - striver
aliases:
  - Maximal Square
  - LeetCode 221
difficulty: Medium
topic: Dynamic Programming / Matrix
pattern: 2D DP — min of three neighbors + 1
leetcode_id: 221
date: 2026-05-19
---

# Maximal Square

## 📌 Problem Statement

> Given an m×n binary matrix filled with '0's and '1's, find the largest square containing only '1's and return its area.

**LeetCode #221** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

`dp[i][j]` = side length of largest square with bottom-right at (i,j). If `matrix[i][j] == '1'`: `dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1`.

---

## ✅ Java Solution

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int m = matrix.length, n = matrix[0].length, maxSide = 0;
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
                if (matrix[i-1][j-1] == '1') {
                    dp[i][j] = Math.min(dp[i-1][j], Math.min(dp[i][j-1], dp[i-1][j-1])) + 1;
                    maxSide = Math.max(maxSide, dp[i][j]);
                }

        return maxSide * maxSide;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m × n) |
| Space | O(m × n) |
