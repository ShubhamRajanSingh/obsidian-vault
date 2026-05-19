---
tags:
  - dsa
  - two-pointer
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Container With Most Water
  - LeetCode 11
difficulty: Medium
topic: Two Pointer / Array
pattern: Two Pointer (Greedy shrink)
leetcode_id: 11
date: 2026-05-19
---

# Container With Most Water

## 📌 Problem Statement

> Given `n` non-negative integers `height[i]` representing vertical lines, find two lines that together with the x-axis form a container that holds the most water.

**LeetCode #11** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Use two pointers at both ends. The area is limited by the shorter line, so move the pointer at the shorter line inward — you can only potentially gain by finding a taller line.

---

## ✅ Java Solution

```java
class Solution {
    public int maxArea(int[] height) {
        int l = 0, r = height.length - 1, max = 0;

        while (l < r) {
            max = Math.max(max, Math.min(height[l], height[r]) * (r - l));
            if (height[l] < height[r]) l++;
            else r--;
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
| Space | O(1) |
