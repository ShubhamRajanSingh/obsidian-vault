---
tags:
  - dsa
  - binary-search
  - array
  - hard
  - leetcode
aliases:
  - Median of Two Sorted Arrays
  - LeetCode 4
difficulty: Hard
topic: Binary Search / Array
pattern: Binary Search on partition
leetcode_id: 4
date: 2026-05-19
---

# Median of Two Sorted Arrays

## 📌 Problem Statement

> Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the **median** of the two sorted arrays. The overall run time complexity should be O(log(m+n)).

**LeetCode #4** | Difficulty: 🔴 Hard

### Example
```
Input:  nums1 = [1,3], nums2 = [2]
Output: 2.0  (merged = [1,2,3])
```

---

## 🧠 Intuition

Binary search on the smaller array. Partition both arrays such that the left halves combined contain the correct number of elements for the median. Ensure `maxLeft1 <= minRight2` and `maxLeft2 <= minRight1`.

---

## ✅ Java Solution

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) return findMedianSortedArrays(nums2, nums1);

        int m = nums1.length, n = nums2.length;
        int lo = 0, hi = m;

        while (lo <= hi) {
            int px = (lo + hi) / 2;
            int py = (m + n + 1) / 2 - px;

            int maxLeftX  = (px == 0) ? Integer.MIN_VALUE : nums1[px - 1];
            int minRightX = (px == m) ? Integer.MAX_VALUE : nums1[px];
            int maxLeftY  = (py == 0) ? Integer.MIN_VALUE : nums2[py - 1];
            int minRightY = (py == n) ? Integer.MAX_VALUE : nums2[py];

            if (maxLeftX <= minRightY && maxLeftY <= minRightX) {
                if ((m + n) % 2 == 1) return Math.max(maxLeftX, maxLeftY);
                return (Math.max(maxLeftX, maxLeftY) + Math.min(minRightX, minRightY)) / 2.0;
            } else if (maxLeftX > minRightY) hi = px - 1;
            else lo = px + 1;
        }

        return -1;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(log(min(m,n))) |
| Space | O(1) |

---

## 🔗 Related

- [[Median of Two sorted array]]
- [[Kth element in two sorted arrays]]
