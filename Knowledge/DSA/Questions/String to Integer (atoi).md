---
tags:
  - dsa
  - string
  - medium
  - leetcode
aliases:
  - String to Integer atoi
  - LeetCode 8
difficulty: Medium
topic: String / Parsing
pattern: State Machine / Edge case handling
leetcode_id: 8
date: 2026-05-19
---

# String to Integer (atoi)

## 📌 Problem Statement

> Implement `myAtoi(string s)` which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function). Handle whitespace, sign, overflow, and non-digit characters.

**LeetCode #8** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

Steps: skip leading whitespace → read optional sign → read digits → clamp to int range.

---

## ✅ Java Solution

```java
class Solution {
    public int myAtoi(String s) {
        int i = 0, n = s.length(), sign = 1;
        long result = 0;

        while (i < n && s.charAt(i) == ' ') i++;
        if (i < n && (s.charAt(i) == '+' || s.charAt(i) == '-')) {
            sign = (s.charAt(i++) == '-') ? -1 : 1;
        }
        while (i < n && Character.isDigit(s.charAt(i))) {
            result = result * 10 + (s.charAt(i++) - '0');
            if (result * sign > Integer.MAX_VALUE) return Integer.MAX_VALUE;
            if (result * sign < Integer.MIN_VALUE) return Integer.MIN_VALUE;
        }

        return (int)(result * sign);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
