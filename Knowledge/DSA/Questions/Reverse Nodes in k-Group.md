---
tags:
  - dsa
  - linked-list
  - hard
  - leetcode
  - striver
aliases:
  - Reverse Nodes in k-Group
  - LeetCode 25
difficulty: Hard
topic: Linked List
pattern: Group reversal with recursion/iteration
leetcode_id: 25
date: 2026-05-19
---

# Reverse Nodes in k-Group

## 📌 Problem Statement

> Given a linked list, reverse the nodes of the list `k` at a time and return its modified list. If the number of nodes is not a multiple of `k`, the remaining nodes stay as is.

**LeetCode #25** | Difficulty: 🔴 Hard | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode curr = head;
        int count = 0;
        while (curr != null && count < k) { curr = curr.next; count++; }
        if (count < k) return head;

        // Reverse k nodes
        ListNode prev = null;
        curr = head;
        for (int i = 0; i < k; i++) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }

        head.next = reverseKGroup(curr, k);
        return prev;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n/k) recursion |
