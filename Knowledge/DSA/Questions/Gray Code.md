---
tags:
  - dsa
  - string
  - medium
  - leetcode
aliases:
  - Gray Code
  - LeetCode 89
difficulty: Medium
topic: Bit Manipulation / Math
pattern: XOR formula
leetcode_id: 89
date: 2026-05-19
---

# Gray Code

## 📌 Problem Statement

> Given an integer `n`, return any valid n-bit Gray code sequence (a sequence where each successive value differs in exactly one bit).

**LeetCode #89** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < (1 << n); i++) {
            res.add(i ^ (i >> 1));
        }
        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(2^n) |
| Space | O(1) extra |
