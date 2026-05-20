---
tags:
  - dsa
  - dynamic-programming
  - heap
  - medium
  - leetcode
aliases:
  - Ugly Number II
  - LeetCode 264
difficulty: Medium
topic: Dynamic Programming / Heap
pattern: Three-pointer DP for ugly numbers
leetcode_id: 264
date: 2026-05-19
---

# Ugly Number II

## 📌 Problem Statement

> An ugly number is a positive integer whose prime factors are limited to 2, 3, and 5. Given an integer `n`, return the `n`th ugly number.

**LeetCode #264** | Difficulty: 🟡 Medium

---

## ✅ Java Solution (Three-pointer DP)

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        int i2 = 0, i3 = 0, i5 = 0;

        for (int i = 1; i < n; i++) {
            int next = Math.min(dp[i2] * 2, Math.min(dp[i3] * 3, dp[i5] * 5));
            dp[i] = next;
            if (next == dp[i2] * 2) i2++;
            if (next == dp[i3] * 3) i3++;
            if (next == dp[i5] * 5) i5++;
        }

        return dp[n - 1];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
