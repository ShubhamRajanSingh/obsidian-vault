---
tags:
  - dsa
  - string
  - dynamic-programming
  - medium
  - leetcode
aliases:
  - Longest Palindromic Substring
  - LeetCode 5
difficulty: Medium
topic: String / DP
pattern: Expand Around Center
leetcode_id: 5
date: 2026-05-19
---

# Longest Palindromic Substring

## 📌 Problem Statement

> Given a string `s`, return the longest palindromic substring in `s`.

**LeetCode #5** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

For each character (and each pair of adjacent characters), expand outward as long as characters match. Track the longest palindrome found.

---

## ✅ Java Solution

```java
class Solution {
    int start = 0, maxLen = 1;

    public String longestPalindrome(String s) {
        for (int i = 0; i < s.length(); i++) {
            expand(s, i, i);     // odd length
            expand(s, i, i + 1); // even length
        }
        return s.substring(start, start + maxLen);
    }

    private void expand(String s, int l, int r) {
        while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
            if (r - l + 1 > maxLen) {
                maxLen = r - l + 1;
                start = l;
            }
            l--; r++;
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n²) |
| Space | O(1) |
