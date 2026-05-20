---
tags:
  - dsa
  - tree
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Path Sum III
  - LeetCode 437
difficulty: Medium
topic: Tree / Prefix Sum / DFS
pattern: DFS with prefix sum hashmap
leetcode_id: 437
date: 2026-05-19
---

# Path Sum III

## 📌 Problem Statement

> Given the root of a binary tree and an integer `targetSum`, return the number of paths where the sum of values along the path equals `targetSum`. The path does not need to start or end at the root or a leaf.

**LeetCode #437** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (Prefix Sum)

```java
class Solution {
    Map<Long, Integer> prefixCount = new HashMap<>();

    public int pathSum(TreeNode root, int targetSum) {
        prefixCount.put(0L, 1);
        return dfs(root, 0L, targetSum);
    }

    private int dfs(TreeNode node, long curr, int target) {
        if (node == null) return 0;
        curr += node.val;
        int count = prefixCount.getOrDefault(curr - target, 0);
        prefixCount.merge(curr, 1, Integer::sum);
        count += dfs(node.left, curr, target) + dfs(node.right, curr, target);
        prefixCount.merge(curr, -1, Integer::sum);
        return count;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
