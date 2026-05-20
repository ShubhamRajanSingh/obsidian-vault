---
tags:
  - dsa
  - dynamic-programming
  - array
  - medium
  - leetcode
  - striver
aliases:
  - House Robber II
  - LeetCode 213
difficulty: Medium
topic: Dynamic Programming
pattern: Run House Robber twice (skip first or last)
leetcode_id: 213
date: 2026-05-19
---

# House Robber II

## 📌 Problem Statement

> All houses are arranged in a circle (first and last are adjacent). Return the maximum amount you can rob without robbing two adjacent houses.

**LeetCode #213** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Since the array is circular, either the first or last house is robbed — but not both. Run House Robber on `[0..n-2]` and `[1..n-1]`, take the max.

---

## ✅ Java Solution

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        return Math.max(robRange(nums, 0, n - 2), robRange(nums, 1, n - 1));
    }

    private int robRange(int[] nums, int lo, int hi) {
        int prev2 = 0, prev1 = 0;
        for (int i = lo; i <= hi; i++) {
            int curr = Math.max(prev1, prev2 + nums[i]);
            prev2 = prev1; prev1 = curr;
        }
        return prev1;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
