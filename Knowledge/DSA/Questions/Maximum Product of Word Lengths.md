---
tags:
  - dsa
  - bit-manipulation
  - medium
  - leetcode
aliases:
  - Maximum Product of Word Lengths
  - LeetCode 318
difficulty: Medium
topic: Bit Manipulation
pattern: Bitmask per word to check overlap in O(1)
leetcode_id: 318
date: 2026-05-19
---

# Maximum Product of Word Lengths

## 📌 Problem Statement

> Given a string array `words`, return the maximum value of `length(word[i]) * length(word[j])` where the two words do not share common letters.

**LeetCode #318** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

Represent each word as a bitmask of 26 bits (one per letter). Two words share no letters iff `mask[i] & mask[j] == 0`.

---

## ✅ Java Solution

```java
class Solution {
    public int maxProduct(String[] words) {
        int n = words.length;
        int[] mask = new int[n];
        for (int i = 0; i < n; i++)
            for (char c : words[i].toCharArray())
                mask[i] |= (1 << (c - 'a'));

        int res = 0;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++)
                if ((mask[i] & mask[j]) == 0)
                    res = Math.max(res, words[i].length() * words[j].length());
        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n² + total chars) |
| Space | O(n) |
