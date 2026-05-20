---
tags:
  - dsa
  - merge-sort
  - binary-indexed-tree
  - hard
  - leetcode
  - striver
aliases:
  - Reverse Pairs
  - LeetCode 493
difficulty: Hard
topic: Merge Sort / Binary Indexed Tree
pattern: Modified merge sort to count inversions
leetcode_id: 493
date: 2026-05-19
---

# Reverse Pairs

## 📌 Problem Statement

> Given an integer array `nums`, return the number of reverse pairs. A reverse pair is a pair `(i, j)` where `0 <= i < j < n` and `nums[i] > 2 * nums[j]`.

**LeetCode #493** | Difficulty: 🔴 Hard | Striver SDE Sheet ✅

---

## ✅ Java Solution (Merge Sort)

```java
class Solution {
    public int reversePairs(int[] nums) {
        return mergeSort(nums, 0, nums.length - 1);
    }

    private int mergeSort(int[] nums, int lo, int hi) {
        if (lo >= hi) return 0;
        int mid = lo + (hi - lo) / 2;
        int count = mergeSort(nums, lo, mid) + mergeSort(nums, mid + 1, hi);

        // Count pairs: nums[i] > 2 * nums[j]
        int j = mid + 1;
        for (int i = lo; i <= mid; i++) {
            while (j <= hi && nums[i] > 2L * nums[j]) j++;
            count += j - (mid + 1);
        }

        // Merge
        int[] temp = new int[hi - lo + 1];
        int l = lo, r = mid + 1, k = 0;
        while (l <= mid && r <= hi)
            temp[k++] = nums[l] <= nums[r] ? nums[l++] : nums[r++];
        while (l <= mid) temp[k++] = nums[l++];
        while (r <= hi)  temp[k++] = nums[r++];
        System.arraycopy(temp, 0, nums, lo, temp.length);

        return count;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log n) |
| Space | O(n) |
