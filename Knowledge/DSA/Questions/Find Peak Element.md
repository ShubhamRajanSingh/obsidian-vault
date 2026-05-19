---
tags:
  - dsa
  - binary-search
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Find Peak Element
  - LeetCode 162
difficulty: Medium
topic: Binary Search
pattern: Binary search on slope
leetcode_id: 162
date: 2026-05-19
---

# Find Peak Element

## 📌 Problem Statement

> A peak element is an element strictly greater than its neighbors. Given an integer array `nums`, find a peak element and return its index. Must run in O(log n).

**LeetCode #162** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int lo = 0, hi = nums.length - 1;

        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            if (nums[mid] > nums[mid + 1]) hi = mid;
            else lo = mid + 1;
        }

        return lo;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |
