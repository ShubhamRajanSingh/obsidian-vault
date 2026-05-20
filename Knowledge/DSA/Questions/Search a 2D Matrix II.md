---
tags:
  - dsa
  - matrix
  - binary-search
  - medium
  - leetcode
  - striver
aliases:
  - Search a 2D Matrix II
  - LeetCode 240
difficulty: Medium
topic: Matrix / Binary Search
pattern: Start from top-right corner
leetcode_id: 240
date: 2026-05-19
---

# Search a 2D Matrix II

## 📌 Problem Statement

> Write an efficient algorithm to search for a value `target` in an m×n matrix where each row is sorted left to right and each column is sorted top to bottom.

**LeetCode #240** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Start at top-right. If current > target, move left. If current < target, move down.

---

## ✅ Java Solution

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = 0, col = matrix[0].length - 1;
        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == target) return true;
            else if (matrix[row][col] > target) col--;
            else row++;
        }
        return false;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m + n) |
| Space | O(1) |
