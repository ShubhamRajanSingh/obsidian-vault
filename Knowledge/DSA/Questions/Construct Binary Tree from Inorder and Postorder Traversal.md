---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Construct Binary Tree from Inorder and Postorder Traversal
  - LeetCode 106
difficulty: Medium
topic: Tree / DFS
pattern: Recursive tree construction
leetcode_id: 106
date: 2026-05-19
---

# Construct Binary Tree from Inorder and Postorder Traversal

## 📌 Problem Statement

> Given two integer arrays `inorder` and `postorder`, construct and return the binary tree.

**LeetCode #106** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    Map<Integer, Integer> inMap = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for (int i = 0; i < inorder.length; i++) inMap.put(inorder[i], i);
        return build(postorder, 0, postorder.length - 1, 0, inorder.length - 1);
    }

    private TreeNode build(int[] post, int postL, int postR, int inL, int inR) {
        if (postL > postR) return null;
        TreeNode root = new TreeNode(post[postR]);
        int idx = inMap.get(post[postR]);
        int leftSize = idx - inL;
        root.left = build(post, postL, postL + leftSize - 1, inL, idx - 1);
        root.right = build(post, postL + leftSize, postR - 1, idx + 1, inR);
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
