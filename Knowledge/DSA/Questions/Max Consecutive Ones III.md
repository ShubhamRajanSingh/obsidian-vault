---
tags:
  - dsa
  - sliding-window
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Max Consecutive Ones III
  - LeetCode 1004
difficulty: Medium
topic: Sliding Window
pattern: Sliding window with at-most-k flips
leetcode_id: 1004
date: 2026-05-19
---

# Max Consecutive Ones III

## 📌 Problem Statement

> Given a binary array `nums` and an integer `k`, return the maximum number of consecutive 1's in the array if you can flip at most `k` 0's.

**LeetCode #1004** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0, zeros = 0, res = 0;

        for (int right = 0; right < nums.length; right++) {
            if (nums[right] == 0) zeros++;
            while (zeros > k) if (nums[left++] == 0) zeros--;
            res = Math.max(res, right - left + 1);
        }

        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
