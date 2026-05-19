---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Validate Binary Search Tree
  - LeetCode 98
difficulty: Medium
topic: Tree / DFS
pattern: Inorder traversal or bounded DFS
leetcode_id: 98
date: 2026-05-19
---

# Validate Binary Search Tree

## 📌 Problem Statement

> Given the root of a binary tree, determine if it is a valid binary search tree (BST). A valid BST: left subtree values < node < right subtree values, both subtrees are valid BSTs.

**LeetCode #98** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean validate(TreeNode node, long min, long max) {
        if (node == null) return true;
        if (node.val <= min || node.val >= max) return false;
        return validate(node.left, min, node.val) && validate(node.right, node.val, max);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(h) — height of tree |
