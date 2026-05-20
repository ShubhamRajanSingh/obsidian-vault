---
tags:
  - dsa
  - tree
  - medium
  - leetcode
  - striver
aliases:
  - Serialize and Deserialize BST
  - LeetCode 449
difficulty: Medium
topic: Tree / Serialization
pattern: Preorder encoding + BST property for decode
leetcode_id: 449
date: 2026-05-19
---

# Serialize and Deserialize BST

## 📌 Problem Statement

> Design an algorithm to serialize and deserialize a binary search tree. Serialization is converting the tree to a string; deserialization is reconstructing the tree from that string.

**LeetCode #449** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
public class Codec {
    public String serialize(TreeNode root) {
        if (root == null) return "";
        StringBuilder sb = new StringBuilder();
        preorder(root, sb);
        return sb.toString();
    }

    private void preorder(TreeNode node, StringBuilder sb) {
        if (node == null) return;
        sb.append(node.val).append(",");
        preorder(node.left, sb);
        preorder(node.right, sb);
    }

    public TreeNode deserialize(String data) {
        if (data.isEmpty()) return null;
        int[] vals = Arrays.stream(data.split(",")).mapToInt(Integer::parseInt).toArray();
        return build(vals, new int[]{0}, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }

    private TreeNode build(int[] vals, int[] idx, int min, int max) {
        if (idx[0] >= vals.length || vals[idx[0]] < min || vals[idx[0]] > max) return null;
        int val = vals[idx[0]++];
        TreeNode node = new TreeNode(val);
        node.left = build(vals, idx, min, val);
        node.right = build(vals, idx, val, max);
        return node;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
