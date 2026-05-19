---
tags:
  - dsa
  - dynamic-programming
  - medium
  - leetcode
  - striver
aliases:
  - Triangle
  - LeetCode 120
difficulty: Medium
topic: Dynamic Programming
pattern: Bottom-up DP on triangle
leetcode_id: 120
date: 2026-05-19
---

# Triangle

## 📌 Problem Statement

> Given a triangle array, return the minimum path sum from top to bottom. At each step you may move to an adjacent number in the row below.

**LeetCode #120** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (Bottom-up)

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] dp = new int[n];
        for (int i = 0; i < n; i++) dp[i] = triangle.get(n - 1).get(i);

        for (int row = n - 2; row >= 0; row--)
            for (int col = 0; col <= row; col++)
                dp[col] = triangle.get(row).get(col) + Math.min(dp[col], dp[col + 1]);

        return dp[0];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n²) |
| Space | O(n) |
