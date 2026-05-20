---
tags:
  - dsa
  - dynamic-programming
  - array
  - medium
  - leetcode
  - striver
aliases:
  - House Robber
  - LeetCode 198
difficulty: Medium
topic: Dynamic Programming
pattern: 1D DP (skip adjacent)
leetcode_id: 198
date: 2026-05-19
---

# House Robber

## 📌 Problem Statement

> You are a robber planning to rob houses along a street. Adjacent houses have security systems connected — you cannot rob two adjacent houses. Given `nums[i]` representing the amount of money in each house, return the maximum amount you can rob.

**LeetCode #198** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

At each house: either skip it (take prev max) or rob it (prev-prev + current). `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`.

---

## ✅ Java Solution

```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        int prev2 = 0, prev1 = 0;
        for (int n : nums) {
            int curr = Math.max(prev1, prev2 + n);
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |

---

## 🔗 Related

- [[House Robber II]]
- [[House Robber III]]
