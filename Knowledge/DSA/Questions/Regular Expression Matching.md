---
tags:
  - dsa
  - dynamic-programming
  - string
  - hard
  - leetcode
aliases:
  - Regular Expression Matching
  - LeetCode 10
difficulty: Hard
topic: Dynamic Programming / String
pattern: 2D DP
leetcode_id: 10
date: 2026-05-19
---

# Regular Expression Matching

## 📌 Problem Statement

> Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'`.
> - `'.'` matches any single character.
> - `'*'` matches zero or more of the preceding element.

**LeetCode #10** | Difficulty: 🔴 Hard

---

## 🧠 Intuition

`dp[i][j]` = true if `s[0..i-1]` matches `p[0..j-1]`.
- If `p[j-1] == '*'`: either skip the `x*` pair (dp[i][j-2]) or match one more char (dp[i-1][j] if chars match).
- Else: match single char and look at dp[i-1][j-1].

---

## ✅ Java Solution

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1];
        dp[0][0] = true;

        for (int j = 1; j <= n; j++) {
            if (p.charAt(j - 1) == '*') dp[0][j] = dp[0][j - 2];
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                char pc = p.charAt(j - 1), sc = s.charAt(i - 1);
                if (pc == '*') {
                    dp[i][j] = dp[i][j - 2];
                    if (p.charAt(j - 2) == '.' || p.charAt(j - 2) == sc) {
                        dp[i][j] |= dp[i - 1][j];
                    }
                } else if (pc == '.' || pc == sc) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
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
