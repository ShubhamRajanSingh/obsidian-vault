---
tags:
  - dsa
  - array
  - matrix
  - medium
  - leetcode
  - striver
aliases:
  - Rotate Image
  - LeetCode 48
difficulty: Medium
topic: Array / Matrix
pattern: Transpose + Reverse rows
leetcode_id: 48
date: 2026-05-19
---

# Rotate Image

## 📌 Problem Statement

> Given an n×n 2D matrix, rotate it 90 degrees clockwise in-place.

**LeetCode #48** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

1. Transpose the matrix (swap `matrix[i][j]` with `matrix[j][i]`).
2. Reverse each row.

---

## ✅ Java Solution

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // Transpose
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = tmp;
            }
        // Reverse each row
        for (int[] row : matrix) {
            int l = 0, r = n - 1;
            while (l < r) { int tmp = row[l]; row[l] = row[r]; row[r] = tmp; l++; r--; }
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n²) |
| Space | O(1) |
