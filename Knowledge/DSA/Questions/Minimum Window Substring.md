---
tags:
  - dsa
  - sliding-window
  - string
  - hard
  - leetcode
  - striver
aliases:
  - Minimum Window Substring
  - LeetCode 76
difficulty: Hard
topic: Sliding Window / String
pattern: Variable sliding window with frequency map
leetcode_id: 76
date: 2026-05-19
---

# Minimum Window Substring

## 📌 Problem Statement

> Given strings `s` and `t`, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included.

**LeetCode #76** | Difficulty: 🔴 Hard | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] need = new int[128];
        for (char c : t.toCharArray()) need[c]++;

        int left = 0, right = 0, have = 0, required = t.length();
        int minLen = Integer.MAX_VALUE, start = 0;

        while (right < s.length()) {
            char c = s.charAt(right++);
            if (need[c] > 0) have++;
            need[c]--;

            while (have == required) {
                if (right - left < minLen) { minLen = right - left; start = left; }
                char l = s.charAt(left++);
                need[l]++;
                if (need[l] > 0) have--;
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(|s| + |t|) |
| Space | O(128) = O(1) |
