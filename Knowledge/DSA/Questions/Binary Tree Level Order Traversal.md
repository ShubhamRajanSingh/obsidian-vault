---
tags:
  - dsa
  - tree
  - bfs
  - medium
  - leetcode
  - striver
aliases:
  - Binary Tree Level Order Traversal
  - LeetCode 102
difficulty: Medium
topic: Tree / BFS
pattern: BFS level-by-level
leetcode_id: 102
date: 2026-05-19
---

# Binary Tree Level Order Traversal

## 📌 Problem Statement

> Given the root of a binary tree, return the level order traversal of its nodes' values (i.e., from left to right, level by level).

**LeetCode #102** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
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
            res.add(level);
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
