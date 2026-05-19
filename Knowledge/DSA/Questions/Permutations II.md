---
tags:
  - dsa
  - backtracking
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Permutations II
  - LeetCode 47
difficulty: Medium
topic: Backtracking
pattern: Backtracking with duplicate skipping
leetcode_id: 47
date: 2026-05-19
---

# Permutations II

## 📌 Problem Statement

> Given a collection of numbers `nums` that might contain duplicates, return all unique permutations.

**LeetCode #47** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        backtrack(nums, new boolean[nums.length], new ArrayList<>(), res);
        return res;
    }

    private void backtrack(int[] nums, boolean[] used, List<Integer> curr, List<List<Integer>> res) {
        if (curr.size() == nums.length) { res.add(new ArrayList<>(curr)); return; }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue; // skip duplicate
            used[i] = true;
            curr.add(nums[i]);
            backtrack(nums, used, curr, res);
            curr.remove(curr.size() - 1);
            used[i] = false;
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n × n!) |
| Space | O(n) |
