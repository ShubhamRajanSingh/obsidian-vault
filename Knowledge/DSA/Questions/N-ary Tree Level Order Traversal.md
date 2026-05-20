---
tags:
  - dsa
  - tree
  - bfs
  - medium
  - leetcode
aliases:
  - N-ary Tree Level Order Traversal
  - LeetCode 429
difficulty: Medium
topic: Tree / BFS
pattern: BFS level-by-level on N-ary tree
leetcode_id: 429
date: 2026-05-19
---

# N-ary Tree Level Order Traversal

## 📌 Problem Statement

> Given an n-ary tree, return the level order traversal of its nodes' values.

**LeetCode #429** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        Queue<Node> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int size = q.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                Node node = q.poll();
                level.add(node.val);
                q.addAll(node.children);
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
