---
tags:
  - dsa
  - array
  - two-pointer
  - medium
  - leetcode
  - striver
aliases:
  - Sort Colors
  - Dutch National Flag
  - LeetCode 75
difficulty: Medium
topic: Array / Two Pointer
pattern: Dutch National Flag (3-way partition)
leetcode_id: 75
date: 2026-05-19
---

# Sort Colors

## 📌 Problem Statement

> Given an array with values 0, 1, and 2, sort them in-place so all 0s come first, then 1s, then 2s.

**LeetCode #75** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition — Dutch National Flag

Use three pointers: `lo` (boundary of 0s), `mid` (current), `hi` (boundary of 2s).

---

## ✅ Java Solution

```java
class Solution {
    public void sortColors(int[] nums) {
        int lo = 0, mid = 0, hi = nums.length - 1;

        while (mid <= hi) {
            if (nums[mid] == 0) {
                swap(nums, lo++, mid++);
            } else if (nums[mid] == 1) {
                mid++;
            } else {
                swap(nums, mid, hi--);
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i]; nums[i] = nums[j]; nums[j] = tmp;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
