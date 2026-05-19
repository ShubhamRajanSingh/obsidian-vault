---
tags:
  - dsa
  - tree
  - bfs
  - easy
  - leetcode
aliases:
  - Binary Tree Level Order Traversal II
  - LeetCode 107
difficulty: Medium
topic: Tree / BFS
pattern: BFS + reverse result
leetcode_id: 107
date: 2026-05-19
---

# Binary Tree Level Order Traversal II

## 📌 Problem Statement

> Given the root of a binary tree, return the bottom-up level order traversal (from leaves to root, left to right per level).

**LeetCode #107** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        LinkedList<List<Integer>> res = new LinkedList<>();
        if (root == null) return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                level.add(node.val);
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
            res.addFirst(level); // prepend for bottom-up
        }

        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
