---
tags:
  - dsa
  - tree
  - dfs
  - dynamic-programming
  - medium
  - leetcode
  - striver
aliases:
  - House Robber III
  - LeetCode 337
difficulty: Medium
topic: Tree / DP / DFS
pattern: Tree DP (rob or skip each node)
leetcode_id: 337
date: 2026-05-19
---

# House Robber III

## 📌 Problem Statement

> The thief has found a new place to rob — a binary tree. Adjacent nodes (parent-child) cannot both be robbed. Return the maximum amount the thief can rob.

**LeetCode #337** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int rob(TreeNode root) {
        int[] res = dfs(root);
        return Math.max(res[0], res[1]);
    }

    // returns [rob_root, skip_root]
    private int[] dfs(TreeNode node) {
        if (node == null) return new int[]{0, 0};
        int[] left = dfs(node.left);
        int[] right = dfs(node.right);
        int rob = node.val + left[1] + right[1];
        int skip = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        return new int[]{rob, skip};
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(h) |
