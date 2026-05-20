---
tags:
  - dsa
  - dynamic-programming
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Coin Change
  - LeetCode 322
difficulty: Medium
topic: Dynamic Programming
pattern: Unbounded knapsack DP
leetcode_id: 322
date: 2026-05-19
---

# Coin Change

## 📌 Problem Statement

> Given an integer array `coins` representing coin denominations and an integer `amount`, return the fewest number of coins needed to make up that amount, or -1 if not possible.

**LeetCode #322** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;

        for (int i = 1; i <= amount; i++)
            for (int coin : coins)
                if (coin <= i) dp[i] = Math.min(dp[i], dp[i - coin] + 1);

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(amount × coins) |
| Space | O(amount) |
