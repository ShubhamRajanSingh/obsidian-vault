---
tags:
  - dsa
  - tree
  - bfs
  - medium
  - leetcode
  - striver
aliases:
  - Binary Tree Right Side View
  - LeetCode 199
difficulty: Medium
topic: Tree / BFS
pattern: BFS, take last node per level
leetcode_id: 199
date: 2026-05-19
---

# Binary Tree Right Side View

## 📌 Problem Statement

> Given the root of a binary tree, imagine standing on the right side of it. Return the values of the nodes you can see ordered from top to bottom.

**LeetCode #199** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                if (i == size - 1) res.add(node.val);
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
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
