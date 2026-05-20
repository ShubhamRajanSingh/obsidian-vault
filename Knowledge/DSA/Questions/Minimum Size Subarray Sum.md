---
tags:
  - dsa
  - sliding-window
  - array
  - medium
  - leetcode
aliases:
  - Minimum Size Subarray Sum
  - LeetCode 209
difficulty: Medium
topic: Sliding Window
pattern: Variable-size sliding window
leetcode_id: 209
date: 2026-05-19
---

# Minimum Size Subarray Sum

## 📌 Problem Statement

> Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a subarray whose sum is >= target. Return 0 if no such subarray exists.

**LeetCode #209** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0, sum = 0, min = Integer.MAX_VALUE;
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum >= target) {
                min = Math.min(min, right - left + 1);
                sum -= nums[left++];
            }
        }
        return min == Integer.MAX_VALUE ? 0 : min;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
