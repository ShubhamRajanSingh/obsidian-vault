---
tags:
  - dsa
  - dynamic-programming
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Maximum Subarray
  - Kadane's Algorithm
  - LeetCode 53
difficulty: Medium
topic: Dynamic Programming / Array
pattern: Kadane's Algorithm
leetcode_id: 53
date: 2026-05-19
---

# Maximum Subarray

## 📌 Problem Statement

> Given an integer array `nums`, find the subarray with the largest sum and return its sum.

**LeetCode #53** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition — Kadane's Algorithm

At each index, decide: is it better to extend the previous subarray or start fresh? `currMax = max(nums[i], currMax + nums[i])`.

---

## ✅ Java Solution

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = nums[0], curr = nums[0];
        for (int i = 1; i < nums.length; i++) {
            curr = Math.max(nums[i], curr + nums[i]);
            maxSum = Math.max(maxSum, curr);
        }
        return maxSum;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
