---
tags:
  - dsa
  - backtracking
  - string
  - medium
  - leetcode
  - striver
aliases:
  - Letter Combinations of a Phone Number
  - LeetCode 17
difficulty: Medium
topic: Backtracking / String
pattern: Backtracking / Recursion
leetcode_id: 17
date: 2026-05-19
---

# Letter Combinations of a Phone Number

## 📌 Problem Statement

> Given a string containing digits from 2-9, return all possible letter combinations that the number could represent (like a telephone keypad).

**LeetCode #17** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList<>();
        if (digits.isEmpty()) return res;
        backtrack(digits, 0, new StringBuilder(), res);
        return res;
    }

    private void backtrack(String digits, int i, StringBuilder curr, List<String> res) {
        if (i == digits.length()) { res.add(curr.toString()); return; }
        for (char c : map[digits.charAt(i) - '0'].toCharArray()) {
            curr.append(c);
            backtrack(digits, i + 1, curr, res);
            curr.deleteCharAt(curr.length() - 1);
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(4^n × n) |
| Space | O(n) |
