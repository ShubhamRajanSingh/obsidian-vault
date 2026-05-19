---
tags:
  - dsa
  - dynamic-programming
  - string
  - hard
  - leetcode
  - striver
aliases:
  - Edit Distance
  - LeetCode 72
difficulty: Hard
topic: Dynamic Programming / String
pattern: 2D DP (classic)
leetcode_id: 72
date: 2026-05-19
---

# Edit Distance

## 📌 Problem Statement

> Given two strings `word1` and `word2`, return the minimum number of operations (insert, delete, replace) required to convert `word1` to `word2`.

**LeetCode #72** | Difficulty: 🔴 Hard | Striver SDE Sheet ✅

---

## 🧠 Intuition

`dp[i][j]` = min operations to convert `word1[0..i-1]` to `word2[0..j-1]`.
- If chars match: `dp[i-1][j-1]`
- Else: `1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])` (delete, insert, replace)

---

## ✅ Java Solution

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 0; i <= m; i++) dp[i][0] = i;
        for (int j = 0; j <= n; j++) dp[0][j] = j;

        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = 1 + Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1]));
            }

        return dp[m][n];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m × n) |
| Space | O(m × n) |
