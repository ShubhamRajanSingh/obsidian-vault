---
tags:
  - dsa
  - greedy
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Gas Station
  - LeetCode 134
difficulty: Medium
topic: Greedy / Array
pattern: Greedy circular traversal
leetcode_id: 134
date: 2026-05-19
---

# Gas Station

## 📌 Problem Statement

> There are `n` gas stations arranged in a circle. You are given two arrays `gas` and `cost`. Return the starting gas station index from which you can travel the circle once, or -1 if not possible.

**LeetCode #134** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

If total gas >= total cost, a solution exists. Greedily find the start: reset tank whenever it goes negative.

---

## ✅ Java Solution

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int total = 0, tank = 0, start = 0;

        for (int i = 0; i < gas.length; i++) {
            int diff = gas[i] - cost[i];
            total += diff;
            tank += diff;
            if (tank < 0) { start = i + 1; tank = 0; }
        }

        return total >= 0 ? start : -1;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
