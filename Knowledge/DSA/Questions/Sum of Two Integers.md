---
tags:
  - dsa
  - bit-manipulation
  - medium
  - leetcode
aliases:
  - Sum of Two Integers
  - LeetCode 371
difficulty: Medium
topic: Bit Manipulation
pattern: XOR for sum, AND+shift for carry
leetcode_id: 371
date: 2026-05-19
---

# Sum of Two Integers

## 📌 Problem Statement

> Given two integers `a` and `b`, return the sum without using `+` or `-` operators.

**LeetCode #371** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

`a ^ b` gives sum without carry. `(a & b) << 1` gives the carry. Repeat until carry is 0.

---

## ✅ Java Solution

```java
class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int carry = (a & b) << 1;
            a = a ^ b;
            b = carry;
        }
        return a;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(1) — at most 32 iterations |
| Space | O(1) |
