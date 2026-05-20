---
tags:
  - dsa
  - sliding-window
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Minimum Operations to Reduce X to Zero
  - LeetCode 1658
difficulty: Medium
topic: Sliding Window / Array
pattern: Find max subarray with sum = total - x
leetcode_id: 1658
date: 2026-05-19
---

# Minimum Operations to Reduce X to Zero

## 📌 Problem Statement

> Given an integer array `nums` and an integer `x`, remove the minimum number of elements from the left or right of the array so the remaining elements sum to `sum(nums) - x`. Return -1 if impossible.

**LeetCode #1658** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Equivalent to finding the longest subarray with sum = `total - x`. Use sliding window.

---

## ✅ Java Solution

```java
class Solution {
    public int minOperations(int[] nums, int x) {
        int target = 0;
        for (int n : nums) target += n;
        target -= x;

        if (target < 0) return -1;
        if (target == 0) return nums.length;

        int left = 0, sum = 0, maxLen = -1;
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum > target && left <= right) sum -= nums[left++];
            if (sum == target) maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen == -1 ? -1 : nums.length - maxLen;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
