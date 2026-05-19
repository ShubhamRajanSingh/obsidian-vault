---
tags:
  - dsa
  - tree
  - linked-list
  - medium
  - leetcode
aliases:
  - Convert Sorted List to Binary Search Tree
  - LeetCode 109
difficulty: Medium
topic: Tree / Linked List
pattern: Slow-fast pointer to find midpoint
leetcode_id: 109
date: 2026-05-19
---

# Convert Sorted List to Binary Search Tree

## 📌 Problem Statement

> Given the head of a singly linked list where the elements are sorted in ascending order, convert it to a height-balanced BST.

**LeetCode #109** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) return null;
        if (head.next == null) return new TreeNode(head.val);

        ListNode prev = null, slow = head, fast = head;
        while (fast != null && fast.next != null) {
            prev = slow; slow = slow.next; fast = fast.next.next;
        }

        prev.next = null; // cut left half
        TreeNode root = new TreeNode(slow.val);
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(slow.next);
        return root;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log n) |
| Space | O(log n) |
