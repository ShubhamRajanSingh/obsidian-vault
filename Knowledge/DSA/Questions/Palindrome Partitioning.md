---
tags:
  - dsa
  - backtracking
  - dynamic-programming
  - string
  - medium
  - leetcode
  - striver
aliases:
  - Palindrome Partitioning
  - LeetCode 131
difficulty: Medium
topic: Backtracking / DP
pattern: Backtracking with palindrome check
leetcode_id: 131
date: 2026-05-19
---

# Palindrome Partitioning

## 📌 Problem Statement

> Given a string `s`, partition `s` such that every substring of the partition is a palindrome. Return all possible palindrome partitioning.

**LeetCode #131** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> res = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(String s, int start, List<String> curr, List<List<String>> res) {
        if (start == s.length()) { res.add(new ArrayList<>(curr)); return; }
        for (int end = start + 1; end <= s.length(); end++) {
            String sub = s.substring(start, end);
            if (isPalin(sub)) {
                curr.add(sub);
                backtrack(s, end, curr, res);
                curr.remove(curr.size() - 1);
            }
        }
    }

    private boolean isPalin(String s) {
        int l = 0, r = s.length() - 1;
        while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
        return true;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n × 2^n) |
| Space | O(n) |
