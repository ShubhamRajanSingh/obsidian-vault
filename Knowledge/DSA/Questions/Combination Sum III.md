---
tags:
  - dsa
  - backtracking
  - medium
  - leetcode
aliases:
  - Combination Sum III
  - LeetCode 216
difficulty: Medium
topic: Backtracking
pattern: Backtracking with fixed count and max digit
leetcode_id: 216
date: 2026-05-19
---

# Combination Sum III

## 📌 Problem Statement

> Find all valid combinations of `k` numbers that sum up to `n`, using only numbers 1-9, each used at most once.

**LeetCode #216** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(k, n, 1, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(int k, int remain, int start, List<Integer> curr, List<List<Integer>> res) {
        if (curr.size() == k && remain == 0) { res.add(new ArrayList<>(curr)); return; }
        if (curr.size() == k || remain <= 0) return;
        for (int i = start; i <= 9; i++) {
            curr.add(i);
            backtrack(k, remain - i, i + 1, curr, res);
            curr.remove(curr.size() - 1);
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(C(9,k) × k) |
| Space | O(k) |
