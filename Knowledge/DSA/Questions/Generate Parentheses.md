---
tags:
  - dsa
  - backtracking
  - string
  - medium
  - leetcode
  - striver
aliases:
  - Generate Parentheses
  - LeetCode 22
difficulty: Medium
topic: Backtracking / String
pattern: Backtracking with constraints
leetcode_id: 22
date: 2026-05-19
---

# Generate Parentheses

## 📌 Problem Statement

> Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

**LeetCode #22** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        backtrack(res, new StringBuilder(), 0, 0, n);
        return res;
    }

    private void backtrack(List<String> res, StringBuilder curr, int open, int close, int n) {
        if (curr.length() == 2 * n) { res.add(curr.toString()); return; }
        if (open < n) {
            curr.append('(');
            backtrack(res, curr, open + 1, close, n);
            curr.deleteCharAt(curr.length() - 1);
        }
        if (close < open) {
            curr.append(')');
            backtrack(res, curr, open, close + 1, n);
            curr.deleteCharAt(curr.length() - 1);
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(4^n / √n) — Catalan number |
| Space | O(n) recursion depth |
