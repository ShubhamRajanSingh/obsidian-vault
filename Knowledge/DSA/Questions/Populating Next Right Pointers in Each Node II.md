---
tags:
  - dsa
  - tree
  - bfs
  - medium
  - leetcode
aliases:
  - Populating Next Right Pointers in Each Node II
  - LeetCode 117
difficulty: Medium
topic: Tree / BFS
pattern: Level-order pointer connection (any binary tree)
leetcode_id: 117
date: 2026-05-19
---

# Populating Next Right Pointers in Each Node II

## 📌 Problem Statement

> Same as LC 116, but for any binary tree (not necessarily perfect). Populate each `next` pointer to the next right node at the same level.

**LeetCode #117** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public Node connect(Node root) {
        Node curr = root;
        while (curr != null) {
            Node dummy = new Node(0);
            Node tail = dummy;
            while (curr != null) {
                if (curr.left != null) { tail.next = curr.left; tail = tail.next; }
                if (curr.right != null) { tail.next = curr.right; tail = tail.next; }
                curr = curr.next;
            }
            curr = dummy.next;
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
