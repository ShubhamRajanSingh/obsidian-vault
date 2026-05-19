---
tags:
  - dsa
  - tree
  - dfs
  - hard
  - leetcode
  - striver
aliases:
  - Binary Tree Maximum Path Sum
  - LeetCode 124
difficulty: Hard
topic: Tree / DFS
pattern: Post-order DFS, track max at each node
leetcode_id: 124
date: 2026-05-19
---

# Binary Tree Maximum Path Sum

## 📌 Problem Statement

> Given the root of a binary tree, return the maximum path sum of any non-empty path. A path is a sequence of nodes where each pair of adjacent nodes has an edge, and each node appears at most once.

**LeetCode #124** | Difficulty: 🔴 Hard | Striver SDE Sheet ✅

---

## 🧠 Intuition

For each node, the best path through it = `node.val + max(0, leftGain) + max(0, rightGain)`. The value returned upward is `node.val + max(0, max(leftGain, rightGain))`.

---

## ✅ Java Solution

```java
class Solution {
    int maxSum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        dfs(root);
        return maxSum;
    }

    private int dfs(TreeNode node) {
        if (node == null) return 0;
        int left = Math.max(0, dfs(node.left));
        int right = Math.max(0, dfs(node.right));
        maxSum = Math.max(maxSum, node.val + left + right);
        return node.val + Math.max(left, right);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(h) |
