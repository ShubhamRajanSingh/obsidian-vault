---
tags:
  - dsa
  - greedy
  - stack
  - medium
  - leetcode
  - striver
aliases:
  - Remove K Digits
  - LeetCode 402
difficulty: Medium
topic: Greedy / Monotonic Stack
pattern: Monotonic increasing stack
leetcode_id: 402
date: 2026-05-19
---

# Remove K Digits

## 📌 Problem Statement

> Given a string `num` representing a non-negative integer and integer `k`, remove `k` digits so the resulting number is the smallest possible. Return it as a string (no leading zeros).

**LeetCode #402** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Use a monotonic increasing stack. Whenever the current digit is smaller than the stack top and we still have removals left, pop the stack (remove larger digit). This greedily minimizes the number from the most significant position.

---

## ✅ Java Solution

```java
class Solution {
    public String removeKdigits(String num, int k) {
        Deque<Character> stack = new ArrayDeque<>();
        for (char c : num.toCharArray()) {
            while (k > 0 && !stack.isEmpty() && stack.peek() > c) {
                stack.pop(); k--;
            }
            stack.push(c);
        }
        while (k-- > 0) stack.pop();

        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) sb.append(stack.pollLast());

        // Remove leading zeros
        int i = 0;
        while (i < sb.length() - 1 && sb.charAt(i) == '0') i++;
        return sb.substring(i);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
