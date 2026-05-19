---
tags:
  - dsa
  - string
  - medium
  - leetcode
  - striver
aliases:
  - Reverse Words in a String
  - LeetCode 151
difficulty: Medium
topic: String
pattern: Split + reverse
leetcode_id: 151
date: 2026-05-19
---

# Reverse Words in a String

## 📌 Problem Statement

> Given an input string `s`, reverse the order of words. A word is defined as a sequence of non-space characters. Multiple spaces should be reduced to a single space, with no leading or trailing spaces.

**LeetCode #151** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public String reverseWords(String s) {
        String[] words = s.trim().split("\\s+");
        StringBuilder sb = new StringBuilder();
        for (int i = words.length - 1; i >= 0; i--) {
            sb.append(words[i]);
            if (i > 0) sb.append(" ");
        }
        return sb.toString();
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
