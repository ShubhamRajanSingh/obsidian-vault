---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Flatten Binary Tree to Linked List
  - LeetCode 114
difficulty: Medium
topic: Tree / DFS
pattern: Morris traversal or preorder flatten
leetcode_id: 114
date: 2026-05-19
---

# Flatten Binary Tree to Linked List

## 📌 Problem Statement

> Given the root of a binary tree, flatten the tree into a linked list in-place (using the right pointers, following preorder traversal).

**LeetCode #114** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (Morris-like)

```java
class Solution {
    public void flatten(TreeNode root) {
        TreeNode curr = root;
        while (curr != null) {
            if (curr.left != null) {
                // Find rightmost of left subtree
                TreeNode rightmost = curr.left;
                while (rightmost.right != null) rightmost = rightmost.right;
                // Connect it to curr's right
                rightmost.right = curr.right;
                curr.right = curr.left;
                curr.left = null;
            }
            curr = curr.right;
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
