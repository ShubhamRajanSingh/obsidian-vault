---
tags:
  - dsa
  - dynamic-programming
  - tree
  - medium
  - leetcode
aliases:
  - Unique Binary Search Trees II
  - LeetCode 95
difficulty: Medium
topic: Dynamic Programming / Tree
pattern: Recursive structure enumeration
leetcode_id: 95
date: 2026-05-19
---

# Unique Binary Search Trees II

## 📌 Problem Statement

> Given an integer `n`, return all structurally unique BSTs which have exactly `n` nodes of unique values from 1 to n.

**LeetCode #95** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        return generate(1, n);
    }

    private List<TreeNode> generate(int start, int end) {
        List<TreeNode> res = new ArrayList<>();
        if (start > end) { res.add(null); return res; }

        for (int root = start; root <= end; root++) {
            for (TreeNode left : generate(start, root - 1)) {
                for (TreeNode right : generate(root + 1, end)) {
                    TreeNode node = new TreeNode(root);
                    node.left = left;
                    node.right = right;
                    res.add(node);
                }
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
| Time | O(Catalan(n) × n) |
| Space | O(Catalan(n) × n) |
