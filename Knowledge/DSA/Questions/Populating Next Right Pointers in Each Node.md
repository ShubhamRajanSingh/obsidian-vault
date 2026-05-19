---
tags:
  - dsa
  - tree
  - bfs
  - medium
  - leetcode
aliases:
  - Populating Next Right Pointers in Each Node
  - LeetCode 116
difficulty: Medium
topic: Tree / BFS
pattern: Level-order pointer connection
leetcode_id: 116
date: 2026-05-19
---

# Populating Next Right Pointers in Each Node

## 📌 Problem Statement

> You have a perfect binary tree where all leaves are at the same level. Populate each `next` pointer to point to its next right node. If no next right node, set to `null`.

**LeetCode #116** | Difficulty: 🟡 Medium

---

## ✅ Java Solution (O(1) space)

```java
class Solution {
    public Node connect(Node root) {
        if (root == null) return null;
        Node leftmost = root;

        while (leftmost.left != null) {
            Node curr = leftmost;
            while (curr != null) {
                curr.left.next = curr.right;
                if (curr.next != null) curr.right.next = curr.next.left;
                curr = curr.next;
            }
            leftmost = leftmost.left;
        }

        return root;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
