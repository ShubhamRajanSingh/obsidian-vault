---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
aliases:
  - Sum Root to Leaf Numbers
  - LeetCode 129
difficulty: Medium
topic: Tree / DFS
pattern: DFS with running number
leetcode_id: 129
date: 2026-05-19
---

# Sum Root to Leaf Numbers

## 📌 Problem Statement

> Given a binary tree where each node contains a digit 0-9, return the total sum of all root-to-leaf numbers (each path forms a number).

**LeetCode #129** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public int sumNumbers(TreeNode root) {
        return dfs(root, 0);
    }

    private int dfs(TreeNode node, int curr) {
        if (node == null) return 0;
        curr = curr * 10 + node.val;
        if (node.left == null && node.right == null) return curr;
        return dfs(node.left, curr) + dfs(node.right, curr);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(h) |
