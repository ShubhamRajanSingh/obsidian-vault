---
tags:
  - dsa
  - tree
  - stack
  - medium
  - leetcode
  - striver
aliases:
  - Binary Search Tree Iterator
  - LeetCode 173
difficulty: Medium
topic: Tree / Stack
pattern: Controlled inorder traversal with stack
leetcode_id: 173
date: 2026-05-19
---

# Binary Search Tree Iterator

## 📌 Problem Statement

> Implement an iterator over a BST. `next()` returns the next smallest number. `hasNext()` returns whether any value exists. Both should run in average O(1) time and O(h) space.

**LeetCode #173** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class BSTIterator {
    Deque<TreeNode> stack = new ArrayDeque<>();

    public BSTIterator(TreeNode root) { pushLeft(root); }

    public int next() {
        TreeNode node = stack.pop();
        pushLeft(node.right);
        return node.val;
    }

    public boolean hasNext() { return !stack.isEmpty(); }

    private void pushLeft(TreeNode node) {
        while (node != null) { stack.push(node); node = node.left; }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(1) amortized |
| Space | O(h) |
