---
tags:
  - dsa
  - math
  - recursion
  - medium
  - leetcode
  - striver
aliases:
  - Pow(x, n)
  - LeetCode 50
difficulty: Medium
topic: Math / Recursion
pattern: Fast Exponentiation
leetcode_id: 50
date: 2026-05-19
---

# Pow(x, n)

## 📌 Problem Statement

> Implement `pow(x, n)`, which calculates `x` raised to the power `n`.

**LeetCode #50** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Use fast exponentiation (binary exponentiation): `x^n = x^(n/2) * x^(n/2)`. Handle negative n by computing `1/x^(-n)`.

---

## ✅ Java Solution

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) { x = 1 / x; N = -N; }
        return fastPow(x, N);
    }

    private double fastPow(double x, long n) {
        if (n == 0) return 1.0;
        double half = fastPow(x, n / 2);
        return (n % 2 == 0) ? half * half : half * half * x;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(log n) |
