---
tags:
  - dsa
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Next Permutation
  - LeetCode 31
difficulty: Medium
topic: Array
pattern: Next permutation algorithm
leetcode_id: 31
date: 2026-05-19
---

# Next Permutation

## 📌 Problem Statement

> Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers. If not possible, rearrange to the lowest possible order (ascending).

**LeetCode #31** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Algorithm

1. Find the largest index `i` such that `nums[i] < nums[i+1]` (rightmost descent).
2. Find the largest index `j > i` such that `nums[i] < nums[j]`.
3. Swap `nums[i]` and `nums[j]`.
4. Reverse the suffix starting at `nums[i+1]`.

---

## ✅ Java Solution

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length, i = n - 2;

        while (i >= 0 && nums[i] >= nums[i + 1]) i--;

        if (i >= 0) {
            int j = n - 1;
            while (nums[j] <= nums[i]) j--;
            int tmp = nums[i]; nums[i] = nums[j]; nums[j] = tmp;
        }

        // Reverse from i+1 to end
        int l = i + 1, r = n - 1;
        while (l < r) {
            int tmp = nums[l]; nums[l] = nums[r]; nums[r] = tmp;
            l++; r--;
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
