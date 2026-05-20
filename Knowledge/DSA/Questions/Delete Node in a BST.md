---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Delete Node in a BST
  - LeetCode 450
difficulty: Medium
topic: Tree / BST
pattern: Recursive BST deletion
leetcode_id: 450
date: 2026-05-19
---

# Delete Node in a BST

## 📌 Problem Statement

> Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference of the (possibly updated) BST.

**LeetCode #450** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Algorithm

Three cases: node is a leaf (remove it), node has one child (replace with child), node has two children (replace value with inorder successor, then delete successor).

---

## ✅ Java Solution

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;
        if (key < root.val) root.left = deleteNode(root.left, key);
        else if (key > root.val) root.right = deleteNode(root.right, key);
        else {
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;
            // find inorder successor (min of right subtree)
            TreeNode succ = root.right;
            while (succ.left != null) succ = succ.left;
            root.val = succ.val;
            root.right = deleteNode(root.right, succ.val);
        }
        return root;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(h) |
| Space | O(h) |
