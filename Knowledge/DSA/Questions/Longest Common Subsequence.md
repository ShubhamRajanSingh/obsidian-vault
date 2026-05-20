---
tags:
  - dsa
  - dynamic-programming
  - string
  - medium
  - leetcode
  - striver
aliases:
  - Longest Common Subsequence
  - LCS
  - LeetCode 1143
difficulty: Medium
topic: Dynamic Programming / String
pattern: Classic 2D DP
leetcode_id: 1143
date: 2026-05-19
---

# Longest Common Subsequence

## 📌 Problem Statement

> Given two strings `text1` and `text2`, return the length of their longest common subsequence. A subsequence is a sequence that appears in the same relative order but not necessarily contiguous.

**LeetCode #1143** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

`dp[i][j]` = LCS of `text1[0..i-1]` and `text2[0..j-1]`.
- If chars match: `dp[i-1][j-1] + 1`
- Else: `max(dp[i-1][j], dp[i][j-1])`

---

## ✅ Java Solution

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i-1) == text2.charAt(j-1)) dp[i][j] = dp[i-1][j-1] + 1;
                else dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
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
