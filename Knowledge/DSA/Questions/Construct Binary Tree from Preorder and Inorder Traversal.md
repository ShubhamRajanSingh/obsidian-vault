---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Construct Binary Tree from Preorder and Inorder Traversal
  - LeetCode 105
difficulty: Medium
topic: Tree / DFS
pattern: Recursive tree construction
leetcode_id: 105
date: 2026-05-19
---

# Construct Binary Tree from Preorder and Inorder Traversal

## 📌 Problem Statement

> Given two integer arrays `preorder` and `inorder`, construct and return the binary tree.

**LeetCode #105** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Preorder[0] is always the root. Find it in inorder to split left and right subtrees. Recurse.

---

## ✅ Java Solution

```java
class Solution {
    Map<Integer, Integer> inMap = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) inMap.put(inorder[i], i);
        return build(preorder, 0, preorder.length - 1, 0, inorder.length - 1);
    }

    private TreeNode build(int[] pre, int preL, int preR, int inL, int inR) {
        if (preL > preR) return null;
        TreeNode root = new TreeNode(pre[preL]);
        int idx = inMap.get(pre[preL]);
        int leftSize = idx - inL;
        root.left = build(pre, preL + 1, preL + leftSize, inL, idx - 1);
        root.right = build(pre, preL + leftSize + 1, preR, idx + 1, inR);
        return root;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
