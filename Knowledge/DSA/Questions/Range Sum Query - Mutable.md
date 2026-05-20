---
tags:
  - dsa
  - dynamic-programming
  - string
  - medium
  - leetcode
aliases:
  - Longest Increasing Subsequence (LC 300 duplicate note)
  - Longest Common Subsequence
  - LeetCode 1143
difficulty: Medium
topic: Dynamic Programming
pattern: 2D DP
leetcode_id: 300
date: 2026-05-19
---

# Range Sum Query - Mutable

## 📌 Problem Statement

> Given an integer array `nums`, handle multiple queries: update an element, and find the sum of elements between indices `left` and `right` inclusive.

**LeetCode #307** | Difficulty: 🟡 Medium

---

## ✅ Java Solution (Binary Indexed Tree / Fenwick Tree)

```java
class NumArray {
    int[] tree, nums;
    int n;

    public NumArray(int[] nums) {
        n = nums.length;
        tree = new int[n + 1];
        this.nums = new int[n];
        for (int i = 0; i < n; i++) update(i, nums[i]);
    }

    public void update(int i, int val) {
        int diff = val - nums[i];
        nums[i] = val;
        for (int j = i + 1; j <= n; j += j & (-j)) tree[j] += diff;
    }

    public int sumRange(int left, int right) {
        return query(right + 1) - query(left);
    }

    private int query(int i) {
        int sum = 0;
        for (; i > 0; i -= i & (-i)) sum += tree[i];
        return sum;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(log n) per update/query |
| Space | O(n) |
