---
tags:
  - dsa
  - dynamic-programming
  - string
  - hard
  - leetcode
aliases:
  - Distinct Subsequences
  - LeetCode 115
difficulty: Hard
topic: Dynamic Programming / String
pattern: 2D DP subsequence counting
leetcode_id: 115
date: 2026-05-19
---

# Distinct Subsequences

## 📌 Problem Statement

> Given two strings `s` and `t`, return the number of distinct subsequences of `s` which equals `t`.

**LeetCode #115** | Difficulty: 🔴 Hard

---

## 🧠 Intuition

`dp[i][j]` = number of ways to form `t[0..j-1]` from `s[0..i-1]`.
- If `s[i-1] == t[j-1]`: `dp[i][j] = dp[i-1][j-1] + dp[i-1][j]`
- Else: `dp[i][j] = dp[i-1][j]`

---

## ✅ Java Solution

```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length(), n = t.length();
        long[][] dp = new long[m + 1][n + 1];
        for (int i = 0; i <= m; i++) dp[i][0] = 1;

        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++) {
                dp[i][j] = dp[i - 1][j];
                if (s.charAt(i - 1) == t.charAt(j - 1)) dp[i][j] += dp[i - 1][j - 1];
            }

        return (int) dp[m][n];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m × n) |
| Space | O(m × n) |
