---
tags:
  - dsa
  - math
  - hard
  - leetcode
aliases:
  - Permutation Sequence
  - LeetCode 60
difficulty: Hard
topic: Math / Factoradic
pattern: Factoradic number system
leetcode_id: 60
date: 2026-05-19
---

# Permutation Sequence

## 📌 Problem Statement

> Given `n` and `k`, return the `k`th permutation sequence of `[1, 2, ..., n]`.

**LeetCode #60** | Difficulty: 🔴 Hard

---

## ✅ Java Solution

```java
class Solution {
    public String getPermutation(int n, int k) {
        int[] factorial = new int[n + 1];
        factorial[0] = 1;
        List<Integer> digits = new ArrayList<>();
        for (int i = 1; i <= n; i++) { factorial[i] = factorial[i - 1] * i; digits.add(i); }

        k--;
        StringBuilder sb = new StringBuilder();
        for (int i = n; i >= 1; i--) {
            int idx = k / factorial[i - 1];
            sb.append(digits.get(idx));
            digits.remove(idx);
            k %= factorial[i - 1];
        }
        return sb.toString();
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n²) |
| Space | O(n) |
