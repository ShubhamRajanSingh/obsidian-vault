---
tags:
  - dsa
  - array
  - matrix
  - medium
  - leetcode
  - striver
aliases:
  - Spiral Matrix
  - LeetCode 54
difficulty: Medium
topic: Array / Matrix
pattern: Layer-by-layer traversal
leetcode_id: 54
date: 2026-05-19
---

# Spiral Matrix

## 📌 Problem Statement

> Given an m×n matrix, return all elements of the matrix in spiral order.

**LeetCode #54** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int top = 0, bottom = matrix.length - 1, left = 0, right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {
            for (int i = left; i <= right; i++) res.add(matrix[top][i]); top++;
            for (int i = top; i <= bottom; i++) res.add(matrix[i][right]); right--;
            if (top <= bottom) { for (int i = right; i >= left; i--) res.add(matrix[bottom][i]); bottom--; }
            if (left <= right) { for (int i = bottom; i >= top; i--) res.add(matrix[i][left]); left++; }
        }

        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m × n) |
| Space | O(1) extra |
