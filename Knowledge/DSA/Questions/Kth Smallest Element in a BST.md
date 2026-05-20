---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Kth Smallest Element in a BST
  - LeetCode 230
difficulty: Medium
topic: Tree / DFS
pattern: Inorder traversal (kth element)
leetcode_id: 230
date: 2026-05-19
---

# Kth Smallest Element in a BST

## 📌 Problem Statement

> Given the root of a BST and an integer `k`, return the `k`th smallest value (1-indexed) of all values in the tree.

**LeetCode #230** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (Iterative Inorder)

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode curr = root;

        while (curr != null || !stack.isEmpty()) {
            while (curr != null) { stack.push(curr); curr = curr.left; }
            curr = stack.pop();
            if (--k == 0) return curr.val;
            curr = curr.right;
        }

        return -1;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(H + k) |
| Space | O(H) |
