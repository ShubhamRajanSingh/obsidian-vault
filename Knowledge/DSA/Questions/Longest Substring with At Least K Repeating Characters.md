---
tags:
  - dsa
  - sliding-window
  - string
  - medium
  - leetcode
aliases:
  - Longest Substring with At Least K Repeating Characters
  - LeetCode 395
difficulty: Medium
topic: Sliding Window / Divide and Conquer
pattern: Divide and conquer on invalid characters
leetcode_id: 395
date: 2026-05-19
---

# Longest Substring with At Least K Repeating Characters

## 📌 Problem Statement

> Given a string `s` and integer `k`, return the length of the longest substring where every character appears at least `k` times.

**LeetCode #395** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

Any character with count < k is an invalid split point. Divide on those characters and recurse. Alternatively use a sliding window with a fixed number of unique characters.

---

## ✅ Java Solution (Divide & Conquer)

```java
class Solution {
    public int longestSubstring(String s, int k) {
        if (s.isEmpty()) return 0;
        int[] freq = new int[26];
        for (char c : s.toCharArray()) freq[c - 'a']++;

        for (int i = 0; i < s.length(); i++) {
            if (freq[s.charAt(i) - 'a'] < k) {
                int left  = longestSubstring(s.substring(0, i), k);
                int right = longestSubstring(s.substring(i + 1), k);
                return Math.max(left, right);
            }
        }
        return s.length();
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n²) worst case |
| Space | O(n) stack |
