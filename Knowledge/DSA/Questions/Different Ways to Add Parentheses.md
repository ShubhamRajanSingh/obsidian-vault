---
tags:
  - dsa
  - divide-and-conquer
  - dynamic-programming
  - medium
  - leetcode
aliases:
  - Different Ways to Add Parentheses
  - LeetCode 241
difficulty: Medium
topic: Divide and Conquer / DP
pattern: Split on each operator, recurse
leetcode_id: 241
date: 2026-05-19
---

# Different Ways to Add Parentheses

## 📌 Problem Statement

> Given a string `expression` of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. You may return the answer in any order.

**LeetCode #241** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<Integer> diffWaysToCompute(String expression) {
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < expression.length(); i++) {
            char c = expression.charAt(i);
            if (c == '+' || c == '-' || c == '*') {
                List<Integer> left = diffWaysToCompute(expression.substring(0, i));
                List<Integer> right = diffWaysToCompute(expression.substring(i + 1));
                for (int l : left)
                    for (int r : right) {
                        if (c == '+') res.add(l + r);
                        else if (c == '-') res.add(l - r);
                        else res.add(l * r);
                    }
            }
        }
        if (res.isEmpty()) res.add(Integer.parseInt(expression));
        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n × Catalan(n)) |
| Space | O(n × Catalan(n)) |
