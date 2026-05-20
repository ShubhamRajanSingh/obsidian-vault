---
tags:
  - dsa
  - array
  - prefix-sum
  - medium
  - leetcode
  - striver
aliases:
  - Binary Subarrays With Sum
  - LeetCode 930
difficulty: Medium
topic: Array / Prefix Sum / HashMap
pattern: Prefix sum count (same as subarray sum = k)
leetcode_id: 930
date: 2026-05-19
---

# Binary Subarrays With Sum

## 📌 Problem Statement

> Given a binary array `nums` and an integer `goal`, return the number of non-empty subarrays with a sum equal to `goal`.

**LeetCode #930** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        Map<Integer, Integer> count = new HashMap<>();
        count.put(0, 1);
        int sum = 0, res = 0;

        for (int n : nums) {
            sum += n;
            res += count.getOrDefault(sum - goal, 0);
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
| Space | O(n) |
