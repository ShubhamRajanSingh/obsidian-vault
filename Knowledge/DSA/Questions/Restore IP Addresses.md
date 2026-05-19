---
tags:
  - dsa
  - backtracking
  - string
  - medium
  - leetcode
aliases:
  - Restore IP Addresses
  - LeetCode 93
difficulty: Medium
topic: Backtracking / String
pattern: Backtracking with constraint validation
leetcode_id: 93
date: 2026-05-19
---

# Restore IP Addresses

## 📌 Problem Statement

> Given a string `s` containing only digits, return all possible valid IP addresses that can be formed by inserting dots into `s`. A valid IP address consists of four integers (0–255), separated by dots, with no leading zeros.

**LeetCode #93** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> res = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(String s, int start, List<String> parts, List<String> res) {
        if (parts.size() == 4 && start == s.length()) {
            res.add(String.join(".", parts)); return;
        }
        if (parts.size() == 4 || start == s.length()) return;

        for (int len = 1; len <= 3; len++) {
            if (start + len > s.length()) break;
            String part = s.substring(start, start + len);
            if (part.length() > 1 && part.charAt(0) == '0') break; // leading zero
            if (Integer.parseInt(part) > 255) break;
            parts.add(part);
            backtrack(s, start + len, parts, res);
            parts.remove(parts.size() - 1);
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(1) — at most 27 combinations |
| Space | O(1) |
