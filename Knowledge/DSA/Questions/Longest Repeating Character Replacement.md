---
tags:
  - dsa
  - sliding-window
  - string
  - medium
  - leetcode
  - striver
aliases:
  - Longest Repeating Character Replacement
  - LeetCode 424
difficulty: Medium
topic: Sliding Window
pattern: Sliding window with max-freq tracking
leetcode_id: 424
date: 2026-05-19
---

# Longest Repeating Character Replacement

## 📌 Problem Statement

> Given a string `s` and integer `k`, you can replace at most `k` characters. Return the length of the longest substring containing the same letter after replacements.

**LeetCode #424** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Sliding window: window is valid if `(window_size - max_freq) <= k`. Expand right freely; shrink left when invalid.

---

## ✅ Java Solution

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] count = new int[26];
        int left = 0, maxFreq = 0, res = 0;

        for (int right = 0; right < s.length(); right++) {
            maxFreq = Math.max(maxFreq, ++count[s.charAt(right) - 'A']);
            while ((right - left + 1) - maxFreq > k) count[s.charAt(left++) - 'A']--;
            res = Math.max(res, right - left + 1);
        }

        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(26) = O(1) |
