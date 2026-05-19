---
tags:
  - dsa
  - greedy
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Jump Game
  - LeetCode 55
difficulty: Medium
topic: Greedy / Array
pattern: Greedy max reach
leetcode_id: 55
date: 2026-05-19
---

# Jump Game

## 📌 Problem Statement

> Given an integer array `nums` where `nums[i]` is your maximum jump length from index `i`, return `true` if you can reach the last index.

**LeetCode #55** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > maxReach) return false;
            maxReach = Math.max(maxReach, i + nums[i]);
        }
        return true;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
