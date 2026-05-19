---
tags:
  - dsa
  - backtracking
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Subsets
  - LeetCode 78
difficulty: Medium
topic: Backtracking / Bit Manipulation
pattern: Backtracking power set
leetcode_id: 78
date: 2026-05-19
---

# Subsets

## 📌 Problem Statement

> Given an integer array `nums` of unique elements, return all possible subsets (the power set).

**LeetCode #78** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(int[] nums, int start, List<Integer> curr, List<List<Integer>> res) {
        res.add(new ArrayList<>(curr));
        for (int i = start; i < nums.length; i++) {
            curr.add(nums[i]);
            backtrack(nums, i + 1, curr, res);
            curr.remove(curr.size() - 1);
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(2^n × n) |
| Space | O(n) |
