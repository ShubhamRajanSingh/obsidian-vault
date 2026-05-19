---
tags:
  - dsa
  - backtracking
  - medium
  - leetcode
aliases:
  - Combinations
  - LeetCode 77
difficulty: Medium
topic: Backtracking
pattern: Backtracking with start index
leetcode_id: 77
date: 2026-05-19
---

# Combinations

## 📌 Problem Statement

> Given two integers `n` and `k`, return all possible combinations of `k` numbers chosen from the range `[1, n]`.

**LeetCode #77** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(1, n, k, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(int start, int n, int k, List<Integer> curr, List<List<Integer>> res) {
        if (curr.size() == k) { res.add(new ArrayList<>(curr)); return; }
        for (int i = start; i <= n - (k - curr.size()) + 1; i++) {
            curr.add(i);
            backtrack(i + 1, n, k, curr, res);
            curr.remove(curr.size() - 1);
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(C(n,k) × k) |
| Space | O(k) |
