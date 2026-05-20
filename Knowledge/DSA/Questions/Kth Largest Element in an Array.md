---
tags:
  - dsa
  - array
  - heap
  - quickselect
  - medium
  - leetcode
  - striver
aliases:
  - Kth Largest Element in an Array
  - LeetCode 215
difficulty: Medium
topic: Array / Heap / QuickSelect
pattern: Min-heap of size K or QuickSelect
leetcode_id: 215
date: 2026-05-19
---

# Kth Largest Element in an Array

## 📌 Problem Statement

> Given an integer array `nums` and an integer `k`, return the `k`th largest element in the array (not kth distinct).

**LeetCode #215** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (Min-Heap)

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int n : nums) {
            pq.offer(n);
            if (pq.size() > k) pq.poll();
        }
        return pq.peek();
    }
}
```

---

## ✅ Java Solution (QuickSelect — avg O(n))

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return quickSelect(nums, 0, nums.length - 1, nums.length - k);
    }

    private int quickSelect(int[] nums, int lo, int hi, int k) {
        int pivot = nums[hi], p = lo;
        for (int i = lo; i < hi; i++) if (nums[i] <= pivot) swap(nums, i, p++);
        swap(nums, p, hi);
        if (p == k) return nums[p];
        return p < k ? quickSelect(nums, p + 1, hi, k) : quickSelect(nums, lo, p - 1, k);
    }

    private void swap(int[] a, int i, int j) { int t = a[i]; a[i] = a[j]; a[j] = t; }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log k) heap / O(n) avg quickselect |
| Space | O(k) heap / O(1) quickselect |
