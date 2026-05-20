---
tags:
  - dsa
  - bit-manipulation
  - medium
  - leetcode
aliases:
  - Single Number III
  - LeetCode 260
difficulty: Medium
topic: Bit Manipulation
pattern: XOR to isolate two distinct numbers
leetcode_id: 260
date: 2026-05-19
---

# Single Number III

## 📌 Problem Statement

> Given an integer array where every element appears exactly twice except for two elements which appear once, find those two elements.

**LeetCode #260** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

XOR all elements to get `xor = a ^ b`. Find any set bit in xor (they differ here). Partition array into two groups by this bit — each group XOR yields one answer.

---

## ✅ Java Solution

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        for (int n : nums) xor ^= n;
        int diff = xor & (-xor); // lowest set bit
        int a = 0;
        for (int n : nums) if ((n & diff) != 0) a ^= n;
        return new int[]{a, xor ^ a};
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
