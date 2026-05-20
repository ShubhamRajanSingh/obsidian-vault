---
tags:
  - dsa
  - sliding-window
  - string
  - medium
  - leetcode
aliases:
  - Repeated DNA Sequences
  - LeetCode 187
difficulty: Medium
topic: Sliding Window / HashSet
pattern: Fixed-size sliding window with hashing
leetcode_id: 187
date: 2026-05-19
---

# Repeated DNA Sequences

## 📌 Problem Statement

> Given a DNA string `s`, return all 10-letter-long substrings that occur more than once.

**LeetCode #187** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set<String> seen = new HashSet<>(), res = new HashSet<>();
        for (int i = 0; i + 10 <= s.length(); i++) {
            String sub = s.substring(i, i + 10);
            if (!seen.add(sub)) res.add(sub);
        }
        return new ArrayList<>(res);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n × 10) = O(n) |
| Space | O(n) |
