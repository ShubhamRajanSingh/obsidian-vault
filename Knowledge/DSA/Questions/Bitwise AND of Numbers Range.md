---
tags:
  - dsa
  - bit-manipulation
  - medium
  - leetcode
aliases:
  - Bitwise AND of Numbers Range
  - LeetCode 201
difficulty: Medium
topic: Bit Manipulation
pattern: Find common prefix bits
leetcode_id: 201
date: 2026-05-19
---

# Bitwise AND of Numbers Range

## 📌 Problem Statement

> Given two integers `left` and `right`, return the bitwise AND of all numbers in the range `[left, right]`.

**LeetCode #201** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

Shift both numbers right until they are equal — this finds the common prefix. Then shift back left by the same amount.

---

## ✅ Java Solution

```java
class Solution {
    public int rangeBitwiseAnd(int left, int right) {
        int shift = 0;
        while (left != right) { left >>= 1; right >>= 1; shift++; }
        return left << shift;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |
