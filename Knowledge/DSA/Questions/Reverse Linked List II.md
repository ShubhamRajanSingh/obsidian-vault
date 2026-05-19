---
tags:
  - dsa
  - linked-list
  - medium
  - leetcode
aliases:
  - Reverse Linked List II
  - LeetCode 92
difficulty: Medium
topic: Linked List
pattern: Reverse a portion in one pass
leetcode_id: 92
date: 2026-05-19
---

# Reverse Linked List II

## 📌 Problem Statement

> Given the head of a singly linked list and two integers `left` and `right`, reverse the nodes from position `left` to position `right`, and return the reversed list.

**LeetCode #92** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;

        for (int i = 1; i < left; i++) prev = prev.next;

        ListNode curr = prev.next;
        for (int i = 0; i < right - left; i++) {
            ListNode next = curr.next;
            curr.next = next.next;
            next.next = prev.next;
            prev.next = next;
        }

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
