---
tags:
  - dsa
  - dynamic-programming
  - tree
  - medium
  - leetcode
  - striver
aliases:
  - Unique Binary Search Trees
  - Catalan Number
  - LeetCode 96
difficulty: Medium
topic: Dynamic Programming / Math
pattern: Catalan number DP
leetcode_id: 96
date: 2026-05-19
---

# Unique Binary Search Trees

## 📌 Problem Statement

> Given an integer `n`, return the number of structurally unique BSTs which have exactly `n` nodes of unique values from 1 to n.

**LeetCode #96** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

`dp[i]` = number of unique BSTs with `i` nodes = sum of `dp[j-1] * dp[i-j]` for each root `j` from 1 to i. This is the Catalan number.

---

## ✅ Java Solution

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = dp[1] = 1;

        for (int i = 2; i <= n; i++)
            for (int j = 1; j <= i; j++)
                dp[i] += dp[j - 1] * dp[i - j];

        return dp[n];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n²) |
| Space | O(n) |
