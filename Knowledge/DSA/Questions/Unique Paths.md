---
tags:
  - dsa
  - dynamic-programming
  - medium
  - leetcode
  - striver
aliases:
  - Unique Paths
  - LeetCode 62
difficulty: Medium
topic: Dynamic Programming
pattern: 2D DP / Combinatorics
leetcode_id: 62
date: 2026-05-19
---

# Unique Paths

## 📌 Problem Statement

> A robot is on an m×n grid starting at top-left. It can only move right or down. How many unique paths are there to reach the bottom-right?

**LeetCode #62** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        for (int i = 1; i < m; i++)
            for (int j = 1; j < n; j++)
                dp[j] += dp[j - 1];

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
