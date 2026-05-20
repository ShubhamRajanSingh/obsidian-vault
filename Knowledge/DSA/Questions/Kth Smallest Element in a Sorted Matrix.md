---
tags:
  - dsa
  - heap
  - binary-search
  - medium
  - leetcode
aliases:
  - Kth Smallest Element in a Sorted Matrix
  - LeetCode 378
difficulty: Medium
topic: Heap / Binary Search
pattern: Binary search on value range
leetcode_id: 378
date: 2026-05-19
---

# Kth Smallest Element in a Sorted Matrix

## 📌 Problem Statement

> Given an n×n matrix where each row and column is sorted in ascending order, return the kth smallest element in the matrix.

**LeetCode #378** | Difficulty: 🟡 Medium

---

## ✅ Java Solution (Binary Search)

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int lo = matrix[0][0], hi = matrix[n-1][n-1];

        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            int count = countLessEqual(matrix, mid, n);
            if (count < k) lo = mid + 1;
            else hi = mid;
        }
        return lo;
    }

    private int countLessEqual(int[][] matrix, int mid, int n) {
        int count = 0, row = n - 1, col = 0;
        while (row >= 0 && col < n) {
            if (matrix[row][col] <= mid) { count += row + 1; col++; }
            else row--;
        }
        return count;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log(max - min)) |
| Space | O(1) |
