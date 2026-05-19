---
tags:
  - dsa
  - string
  - simulation
  - easy
  - leetcode
aliases:
  - Count and Say
  - LeetCode 38
difficulty: Medium
topic: String / Simulation
pattern: Simulation
leetcode_id: 38
date: 2026-05-19
---

# Count and Say

## 📌 Problem Statement

> The count-and-say sequence: `1, 11, 21, 1211, 111221, ...`. Each term describes the previous term. Return the `n`th term.

**LeetCode #38** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public String countAndSay(int n) {
        String s = "1";
        for (int i = 1; i < n; i++) {
            StringBuilder sb = new StringBuilder();
            int j = 0;
            while (j < s.length()) {
                char c = s.charAt(j);
                int count = 0;
                while (j < s.length() && s.charAt(j) == c) { j++; count++; }
                sb.append(count).append(c);
            }
            s = sb.toString();
        }
        return s;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n × length) |
| Space | O(length) |
