---
tags:
  - dsa
  - string
  - medium
  - leetcode
aliases:
  - Zigzag Conversion
  - LeetCode 6
difficulty: Medium
topic: String
pattern: Simulation / Row assignment
leetcode_id: 6
date: 2026-05-19
---

# Zigzag Conversion

## 📌 Problem Statement

> The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows, then read line by line. Given the string and number of rows, return the converted string.

**LeetCode #6** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1) return s;

        StringBuilder[] rows = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) rows[i] = new StringBuilder();

        int row = 0, dir = -1;
        for (char c : s.toCharArray()) {
            rows[row].append(c);
            if (row == 0 || row == numRows - 1) dir = -dir;
            row += dir;
        }

        StringBuilder res = new StringBuilder();
        for (StringBuilder sb : rows) res.append(sb);
        return res.toString();
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
