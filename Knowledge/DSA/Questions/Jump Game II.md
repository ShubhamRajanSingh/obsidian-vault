---
tags:
  - dsa
  - greedy
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Jump Game II
  - LeetCode 45
difficulty: Medium
topic: Greedy / Array
pattern: Greedy BFS-like jumps
leetcode_id: 45
date: 2026-05-19
---

# Jump Game II

## 📌 Problem Statement

> Given an array `nums` where `nums[i]` is the max jump length from index `i`, return the minimum number of jumps to reach the last index.

**LeetCode #45** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int jump(int[] nums) {
        int jumps = 0, curEnd = 0, farthest = 0;

        for (int i = 0; i < nums.length - 1; i++) {
            farthest = Math.max(farthest, i + nums[i]);
            if (i == curEnd) {
                jumps++;
                curEnd = farthest;
            }
        }

        return jumps;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
