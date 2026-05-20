---
tags:
  - dsa
  - array
  - medium
  - leetcode
aliases:
  - Majority Element II
  - LeetCode 229
difficulty: Medium
topic: Array / Boyer-Moore Voting
pattern: Boyer-Moore Voting (extended to 2 candidates)
leetcode_id: 229
date: 2026-05-19
---

# Majority Element II

## 📌 Problem Statement

> Given an integer array of size `n`, find all elements that appear more than `n/3` times.

**LeetCode #229** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

At most 2 elements can appear more than n/3 times. Use Boyer-Moore voting with 2 candidates, then verify their actual counts.

---

## ✅ Java Solution

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int c1 = 0, c2 = 1, cnt1 = 0, cnt2 = 0;
        for (int n : nums) {
            if (n == c1) cnt1++;
            else if (n == c2) cnt2++;
            else if (cnt1 == 0) { c1 = n; cnt1 = 1; }
            else if (cnt2 == 0) { c2 = n; cnt2 = 1; }
            else { cnt1--; cnt2--; }
        }
        cnt1 = 0; cnt2 = 0;
        for (int n : nums) { if (n == c1) cnt1++; else if (n == c2) cnt2++; }
        List<Integer> res = new ArrayList<>();
        if (cnt1 > nums.length / 3) res.add(c1);
        if (cnt2 > nums.length / 3) res.add(c2);
        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
