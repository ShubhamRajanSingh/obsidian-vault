---
tags:
  - dsa
  - dynamic-programming
  - string
  - hard
  - leetcode
aliases:
  - Wildcard Matching
  - LeetCode 44
difficulty: Hard
topic: Dynamic Programming / String
pattern: 2D DP
leetcode_id: 44
date: 2026-05-19
---

# Wildcard Matching

## 📌 Problem Statement

> Given an input string `s` and a pattern `p`, implement wildcard pattern matching with support for `'?'` (matches any single char) and `'*'` (matches any sequence including empty).

**LeetCode #44** | Difficulty: 🔴 Hard

---

## ✅ Java Solution

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1];
        dp[0][0] = true;

        for (int j = 1; j <= n; j++) dp[0][j] = dp[0][j - 1] && p.charAt(j - 1) == '*';

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                char pc = p.charAt(j - 1);
                if (pc == '*') dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
                else if (pc == '?' || pc == s.charAt(i - 1)) dp[i][j] = dp[i - 1][j - 1];
            }
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
