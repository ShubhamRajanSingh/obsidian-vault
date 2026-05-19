---
tags:
  - dsa
  - dynamic-programming
  - stack
  - hard
  - leetcode
aliases:
  - Longest Valid Parentheses
  - LeetCode 32
difficulty: Hard
topic: DP / Stack
pattern: Stack-based or DP
leetcode_id: 32
date: 2026-05-19
---

# Longest Valid Parentheses

## 📌 Problem Statement

> Given a string containing just `'('` and `')'`, return the length of the longest valid (well-formed) parentheses substring.

**LeetCode #32** | Difficulty: 🔴 Hard

---

## ✅ Java Solution (Stack)

```java
class Solution {
    public int longestValidParentheses(String s) {
        Deque<Integer> stack = new ArrayDeque<>();
        stack.push(-1);
        int max = 0;

        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.isEmpty()) stack.push(i);
                else max = Math.max(max, i - stack.peek());
            }
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
| Space | O(n) |
