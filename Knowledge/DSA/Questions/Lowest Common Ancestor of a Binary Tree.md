---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Lowest Common Ancestor of a Binary Tree
  - LeetCode 236
difficulty: Medium
topic: Tree / DFS
pattern: Post-order DFS LCA
leetcode_id: 236
date: 2026-05-19
---

# Lowest Common Ancestor of a Binary Tree

## 📌 Problem Statement

> Given a binary tree, find the lowest common ancestor (LCA) of two given nodes `p` and `q`.

**LeetCode #236** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

If the current node is `p` or `q`, return it. Otherwise recurse left and right. If both sides return non-null, current node is the LCA. Otherwise return the non-null side.

---

## ✅ Java Solution

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) return root;
        return left != null ? left : right;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(h) |
