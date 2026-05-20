---
tags:
  - dsa
  - dynamic-programming
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Partition Equal Subset Sum
  - LeetCode 416
difficulty: Medium
topic: Dynamic Programming / 0-1 Knapsack
pattern: 0/1 Knapsack — subset sum
leetcode_id: 416
date: 2026-05-19
---

# Partition Equal Subset Sum

## 📌 Problem Statement

> Given a non-empty array `nums`, determine if it can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**LeetCode #416** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

If total sum is odd, impossible. Otherwise find a subset with sum = total/2 — classic 0/1 knapsack.

---

## ✅ Java Solution

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int n : nums) sum += n;
        if (sum % 2 != 0) return false;
        int target = sum / 2;

        boolean[] dp = new boolean[target + 1];
        dp[0] = true;

        for (int n : nums)
            for (int j = target; j >= n; j--)
                dp[j] |= dp[j - n];

        return dp[target];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n × target) |
| Space | O(target) |
