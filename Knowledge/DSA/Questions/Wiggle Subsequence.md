---
tags:
  - dsa
  - greedy
  - medium
  - leetcode
aliases:
  - Wiggle Subsequence
  - LeetCode 376
difficulty: Medium
topic: Greedy / Dynamic Programming
pattern: Greedy peak-valley counting
leetcode_id: 376
date: 2026-05-19
---

# Wiggle Subsequence

## 📌 Problem Statement

> A wiggle sequence is one where successive differences alternate between positive and negative. Return the length of the longest wiggle subsequence of `nums`.

**LeetCode #376** | Difficulty: 🟡 Medium

---

## ✅ Java Solution (Greedy)

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int up = 1, down = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i-1]) up = down + 1;
            else if (nums[i] < nums[i-1]) down = up + 1;
        }
        return Math.max(up, down);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
