---
tags:
  - dsa
  - backtracking
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Combination Sum II
  - LeetCode 40
difficulty: Medium
topic: Backtracking
pattern: Backtracking without reuse, skip duplicates
leetcode_id: 40
date: 2026-05-19
---

# Combination Sum II

## 📌 Problem Statement

> Given a collection of candidate numbers (may have duplicates) and a `target`, find all unique combinations where the candidate numbers sum to target. Each number may only be used once.

**LeetCode #40** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> res = new ArrayList<>();
        backtrack(candidates, target, 0, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(int[] cands, int remain, int start, List<Integer> curr, List<List<Integer>> res) {
        if (remain == 0) { res.add(new ArrayList<>(curr)); return; }
        for (int i = start; i < cands.length && cands[i] <= remain; i++) {
            if (i > start && cands[i] == cands[i - 1]) continue; // skip duplicates
            curr.add(cands[i]);
            backtrack(cands, remain - cands[i], i + 1, curr, res);
            curr.remove(curr.size() - 1);
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(2^n) |
| Space | O(n) |
