---
tags:
  - dsa
  - sliding-window
  - hashmap
  - medium
  - leetcode
aliases:
  - Longest Substring Without Repeating Characters
  - LeetCode 3
difficulty: Medium
topic: Sliding Window / HashMap
pattern: Sliding Window
leetcode_id: 3
date: 2026-05-19
---

# Longest Substring Without Repeating Characters

## 📌 Problem Statement

> Given a string `s`, find the length of the longest substring without repeating characters.

**LeetCode #3** | Difficulty: 🟡 Medium

### Example
```
Input:  s = "abcabcbb"
Output: 3  ("abc")
```

---

## 🧠 Intuition

Use a sliding window with a HashSet. Expand right pointer; when a duplicate is found, shrink from the left until the duplicate is removed.

---

## ✅ Java Solution

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> map = new HashMap<>();
        int max = 0, left = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (map.containsKey(c)) {
                left = Math.max(left, map.get(c) + 1);
            }
            map.put(c, right);
            max = Math.max(max, right - left + 1);
        }

        return max;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(min(n, charset)) |

---

## 🔗 Related

- [[Sliding Window Problem]]
- [[Longest Repeating Character Replacement]]
