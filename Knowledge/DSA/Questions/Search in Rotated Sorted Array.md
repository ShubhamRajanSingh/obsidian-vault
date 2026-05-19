---
tags:
  - dsa
  - binary-search
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Search in Rotated Sorted Array
  - LeetCode 33
difficulty: Medium
topic: Binary Search
pattern: Modified Binary Search
leetcode_id: 33
date: 2026-05-19
---

# Search in Rotated Sorted Array

## 📌 Problem Statement

> There is an integer array `nums` sorted in ascending order (with distinct values), possibly rotated at an unknown pivot. Given `target`, return its index or -1 if not found.

**LeetCode #33** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

At any mid point, one half is always sorted. Check which half is sorted, then decide which half to search.

---

## ✅ Java Solution

```java
class Solution {
    public int search(int[] nums, int target) {
        int lo = 0, hi = nums.length - 1;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (nums[mid] == target) return mid;

            if (nums[lo] <= nums[mid]) { // left half sorted
                if (target >= nums[lo] && target < nums[mid]) hi = mid - 1;
                else lo = mid + 1;
            } else { // right half sorted
                if (target > nums[mid] && target <= nums[hi]) lo = mid + 1;
                else hi = mid - 1;
            }
        }

        return -1;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |
