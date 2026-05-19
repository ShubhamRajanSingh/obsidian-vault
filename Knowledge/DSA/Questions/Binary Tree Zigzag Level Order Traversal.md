---
tags:
  - dsa
  - tree
  - bfs
  - medium
  - leetcode
aliases:
  - Binary Tree Zigzag Level Order Traversal
  - LeetCode 103
difficulty: Medium
topic: Tree / BFS
pattern: BFS with alternating direction
leetcode_id: 103
date: 2026-05-19
---

# Binary Tree Zigzag Level Order Traversal

## 📌 Problem Statement

> Given the root of a binary tree, return the zigzag level order traversal of its nodes' values (alternating left-to-right and right-to-left level by level).

**LeetCode #103** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        boolean leftToRight = true;

        while (!q.isEmpty()) {
            int size = q.size();
            LinkedList<Integer> level = new LinkedList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                if (leftToRight) level.addLast(node.val);
                else level.addFirst(node.val);
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
            res.add(level);
            leftToRight = !leftToRight;
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
