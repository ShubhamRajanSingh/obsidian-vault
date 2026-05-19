---
tags:
  - dsa
  - backtracking
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Combination Sum
  - LeetCode 39
difficulty: Medium
topic: Backtracking
pattern: Backtracking with reuse
leetcode_id: 39
date: 2026-05-19
---

# Combination Sum

## 📌 Problem Statement

> Given an array of distinct integers `candidates` and a `target`, return all unique combinations of candidates where the chosen numbers sum to target. Same number may be chosen unlimited times.

**LeetCode #39** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(candidates);
        backtrack(candidates, target, 0, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(int[] cands, int remain, int start, List<Integer> curr, List<List<Integer>> res) {
        if (remain == 0) { res.add(new ArrayList<>(curr)); return; }
        for (int i = start; i < cands.length && cands[i] <= remain; i++) {
            curr.add(cands[i]);
            backtrack(cands, remain - cands[i], i, curr, res); // i, not i+1 (reuse allowed)
            curr.remove(curr.size() - 1);
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n^(T/M)) where T=target, M=min candidate |
| Space | O(T/M) |
