---
tags:
  - dsa
  - dynamic-programming
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Longest Increasing Subsequence
  - LIS
  - LeetCode 300
difficulty: Medium
topic: Dynamic Programming / Binary Search
pattern: DP O(n²) or patience sorting O(n log n)
leetcode_id: 300
date: 2026-05-19
---

# Longest Increasing Subsequence

## 📌 Problem Statement

> Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

**LeetCode #300** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (O(n log n) — Patience Sort)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        List<Integer> tails = new ArrayList<>();
        for (int n : nums) {
            int lo = 0, hi = tails.size();
            while (lo < hi) {
                int mid = lo + (hi - lo) / 2;
                if (tails.get(mid) < n) lo = mid + 1;
                else hi = mid;
            }
            if (lo == tails.size()) tails.add(n);
            else tails.set(lo, n);
        }
        return tails.size();
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log n) |
| Space | O(n) |
