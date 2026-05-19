---
tags:
  - dsa
  - dynamic-programming
  - medium
  - leetcode
aliases:
  - Unique Paths II
  - LeetCode 63
difficulty: Medium
topic: Dynamic Programming
pattern: 2D DP with obstacles
leetcode_id: 63
date: 2026-05-19
---

# Unique Paths II

## 📌 Problem Statement

> Same as Unique Paths, but some cells contain obstacles (`1`). Return the number of unique paths.

**LeetCode #63** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] dp = new int[n];
        dp[0] = grid[0][0] == 1 ? 0 : 1;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) { dp[j] = 0; }
                else if (j > 0) { dp[j] += dp[j - 1]; }
            }
        }

        return dp[n - 1];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m × n) |
| Space | O(n) |
