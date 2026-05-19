---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Path Sum II
  - LeetCode 113
difficulty: Medium
topic: Tree / DFS / Backtracking
pattern: DFS path tracking
leetcode_id: 113
date: 2026-05-19
---

# Path Sum II

## 📌 Problem Statement

> Given the root of a binary tree and an integer `targetSum`, return all root-to-leaf paths where the sum of node values equals `targetSum`.

**LeetCode #113** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(root, targetSum, new ArrayList<>(), res);
        return res;
    }

    private void dfs(TreeNode node, int remain, List<Integer> path, List<List<Integer>> res) {
        if (node == null) return;
        path.add(node.val);
        if (node.left == null && node.right == null && remain == node.val) {
            res.add(new ArrayList<>(path));
        }
        dfs(node.left, remain - node.val, path, res);
        dfs(node.right, remain - node.val, path, res);
        path.remove(path.size() - 1);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n²) worst case (copying paths) |
| Space | O(h) |
