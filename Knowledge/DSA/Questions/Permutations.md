---
tags:
  - dsa
  - backtracking
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Permutations
  - LeetCode 46
difficulty: Medium
topic: Backtracking
pattern: Backtracking / Swap
leetcode_id: 46
date: 2026-05-19
---

# Permutations

## 📌 Problem Statement

> Given an array `nums` of distinct integers, return all possible permutations.

**LeetCode #46** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(nums, 0, res);
        return res;
    }

    private void backtrack(int[] nums, int start, List<List<Integer>> res) {
        if (start == nums.length) {
            List<Integer> list = new ArrayList<>();
            for (int n : nums) list.add(n);
            res.add(list);
            return;
        }
        for (int i = start; i < nums.length; i++) {
            swap(nums, start, i);
            backtrack(nums, start + 1, res);
            swap(nums, start, i);
        }
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i]; nums[i] = nums[j]; nums[j] = tmp;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n × n!) |
| Space | O(n) |
