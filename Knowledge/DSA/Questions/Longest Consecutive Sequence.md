---
tags:
  - dsa
  - array
  - hashset
  - medium
  - leetcode
  - striver
aliases:
  - Longest Consecutive Sequence
  - LeetCode 128
difficulty: Medium
topic: Array / HashSet
pattern: HashSet O(n) lookup
leetcode_id: 128
date: 2026-05-19
---

# Longest Consecutive Sequence

## 📌 Problem Statement

> Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence. Must run in O(n).

**LeetCode #128** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Add all numbers to a HashSet. For each number that is the start of a sequence (no `num-1` in set), count how long the streak extends.

---

## ✅ Java Solution

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int n : nums) set.add(n);
        int max = 0;

        for (int n : set) {
            if (!set.contains(n - 1)) { // start of sequence
                int len = 1;
                while (set.contains(n + len)) len++;
                max = Math.max(max, len);
            }
        }

        return max;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
