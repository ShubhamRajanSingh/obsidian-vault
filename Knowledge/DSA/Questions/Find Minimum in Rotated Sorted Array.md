---
tags:
  - dsa
  - binary-search
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Find Minimum in Rotated Sorted Array
  - LeetCode 153
difficulty: Medium
topic: Binary Search
pattern: Modified binary search on rotated array
leetcode_id: 153
date: 2026-05-19
---

# Find Minimum in Rotated Sorted Array

## 📌 Problem Statement

> Given a sorted array of unique elements that has been rotated, find the minimum element. Must run in O(log n).

**LeetCode #153** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int findMin(int[] nums) {
        int lo = 0, hi = nums.length - 1;

        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            if (nums[mid] > nums[hi]) lo = mid + 1;
            else hi = mid;
        }

        return nums[lo];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |
