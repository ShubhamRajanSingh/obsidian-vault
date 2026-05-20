---
tags:
  - dsa
  - bit-manipulation
  - medium
  - leetcode
aliases:
  - Total Hamming Distance
  - LeetCode 477
difficulty: Medium
topic: Bit Manipulation
pattern: Count set bits per bit position
leetcode_id: 477
date: 2026-05-19
---

# Total Hamming Distance

## 📌 Problem Statement

> The Hamming distance between two integers is the number of positions at which the corresponding bits differ. Given an integer array `nums`, return the sum of Hamming distances between all pairs of integers.

**LeetCode #477** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

For each of the 32 bit positions, count how many numbers have a 1 (`ones`). The contribution of this bit position to total Hamming distance is `ones * (n - ones)`.

---

## ✅ Java Solution

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int total = 0, n = nums.length;
        for (int i = 0; i < 32; i++) {
            int ones = 0;
            for (int num : nums) ones += (num >> i) & 1;
            total += ones * (n - ones);
        }
        return total;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(32 × n) = O(n) |
| Space | O(1) |
