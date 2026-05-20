---
tags:
  - dsa
  - stack
  - string
  - medium
  - leetcode
  - striver
aliases:
  - Basic Calculator II
  - LeetCode 227
difficulty: Medium
topic: Stack / String
pattern: Stack-based arithmetic evaluation
leetcode_id: 227
date: 2026-05-19
---

# Basic Calculator II

## 📌 Problem Statement

> Given a string `s` representing an expression with `+`, `-`, `*`, `/` and integers, evaluate and return its value. Integer division truncates toward zero.

**LeetCode #227** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int calculate(String s) {
        Deque<Integer> stack = new ArrayDeque<>();
        int num = 0;
        char op = '+';

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) num = num * 10 + (c - '0');
            if ((!Character.isDigit(c) && c != ' ') || i == s.length() - 1) {
                if (op == '+') stack.push(num);
                else if (op == '-') stack.push(-num);
                else if (op == '*') stack.push(stack.pop() * num);
                else if (op == '/') stack.push(stack.pop() / num);
                op = c; num = 0;
            }
        }

        int res = 0;
        while (!stack.isEmpty()) res += stack.pop();
        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
