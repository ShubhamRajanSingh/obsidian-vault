---
tags:
  - dsa
  - linked-list
  - two-pointer
  - medium
  - leetcode
  - striver
aliases:
  - Remove Nth Node From End of List
  - LeetCode 19
difficulty: Medium
topic: Linked List / Two Pointer
pattern: Fast & Slow Pointer
leetcode_id: 19
date: 2026-05-19
---

# Remove Nth Node From End of List

## 📌 Problem Statement

> Given the head of a linked list, remove the `n`th node from the end of the list and return its head.

**LeetCode #19** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Use two pointers. Advance fast pointer n steps ahead. Then move both until fast reaches the end — slow will be just before the node to remove.

---

## ✅ Java Solution

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode fast = dummy, slow = dummy;

        for (int i = 0; i <= n; i++) fast = fast.next;

        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }

        slow.next = slow.next.next;
        return dummy.next;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
