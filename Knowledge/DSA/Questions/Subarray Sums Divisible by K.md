---
tags:
  - dsa
  - array
  - prefix-sum
  - medium
  - leetcode
  - striver
aliases:
  - Subarray Sums Divisible by K
  - LeetCode 974
difficulty: Medium
topic: Array / Prefix Sum / HashMap
pattern: Prefix sum mod k with hashmap
leetcode_id: 974
date: 2026-05-19
---

# Subarray Sums Divisible by K

## 📌 Problem Statement

> Given an integer array `nums` and an integer `k`, return the number of non-empty subarrays that have a sum divisible by `k`.

**LeetCode #974** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

If two prefix sums have the same remainder mod k, the subarray between them is divisible by k.

---

## ✅ Java Solution

```java
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        Map<Integer, Integer> count = new HashMap<>();
        count.put(0, 1);
        int sum = 0, res = 0;

        for (int n : nums) {
            sum = ((sum + n) % k + k) % k; // handle negative
            res += count.getOrDefault(sum, 0);
            count.merge(sum, 1, Integer::sum);
        }

        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(k) |
