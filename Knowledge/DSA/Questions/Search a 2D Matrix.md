---
tags:
  - dsa
  - binary-search
  - matrix
  - medium
  - leetcode
  - striver
aliases:
  - Search a 2D Matrix
  - LeetCode 74
difficulty: Medium
topic: Binary Search / Matrix
pattern: Treat matrix as 1D sorted array
leetcode_id: 74
date: 2026-05-19
---

# Search a 2D Matrix

## 📌 Problem Statement

> Given an m×n matrix where each row is sorted and the first integer of each row is greater than the last integer of the previous row, search for a target value.

**LeetCode #74** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int lo = 0, hi = m * n - 1;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            int val = matrix[mid / n][mid % n];
            if (val == target) return true;
            else if (val < target) lo = mid + 1;
            else hi = mid - 1;
        }

        return false;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(log(m × n)) |
| Space | O(1) |
