---
tags:
  - dsa
  - dynamic-programming
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Maximum Product Subarray
  - LeetCode 152
difficulty: Medium
topic: Dynamic Programming / Array
pattern: Track max and min product
leetcode_id: 152
date: 2026-05-19
---

# Maximum Product Subarray

## 📌 Problem Statement

> Given an integer array `nums`, find a subarray that has the largest product, and return the product.

**LeetCode #152** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Track both `maxProd` and `minProd` at each index (since a negative × negative = positive). At each step, the max could be: current element alone, max × current, or min × current.

---

## ✅ Java Solution

```java
class Solution {
    public int maxProduct(int[] nums) {
        int maxP = nums[0], minP = nums[0], res = nums[0];

        for (int i = 1; i < nums.length; i++) {
            int tmp = maxP;
            maxP = Math.max(nums[i], Math.max(maxP * nums[i], minP * nums[i]));
            minP = Math.min(nums[i], Math.min(tmp * nums[i], minP * nums[i]));
            res = Math.max(res, maxP);
        }

        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
