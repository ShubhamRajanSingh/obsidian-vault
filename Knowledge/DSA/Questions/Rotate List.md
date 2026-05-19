---
tags:
  - dsa
  - linked-list
  - medium
  - leetcode
aliases:
  - Rotate List
  - LeetCode 61
difficulty: Medium
topic: Linked List
pattern: Find tail + reconnect
leetcode_id: 61
date: 2026-05-19
---

# Rotate List

## 📌 Problem Statement

> Given the head of a linked list, rotate the list to the right by `k` places.

**LeetCode #61** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;

        ListNode tail = head;
        int len = 1;
        while (tail.next != null) { tail = tail.next; len++; }

        k = k % len;
        if (k == 0) return head;

        tail.next = head; // make circular
        ListNode newTail = head;
        for (int i = 0; i < len - k - 1; i++) newTail = newTail.next;

        ListNode newHead = newTail.next;
        newTail.next = null;
        return newHead;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
