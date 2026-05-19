---
tags:
  - dsa
  - linked-list
  - two-pointer
  - medium
  - leetcode
  - striver
aliases:
  - Linked List Cycle II
  - LeetCode 142
difficulty: Medium
topic: Linked List / Two Pointer
pattern: Floyd's Cycle Detection
leetcode_id: 142
date: 2026-05-19
---

# Linked List Cycle II

## 📌 Problem Statement

> Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

**LeetCode #142** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition — Floyd's Algorithm

1. Detect cycle using fast/slow pointers.
2. Once they meet, reset `slow` to `head`. Move both one step at a time — they meet at the cycle start.

---

## ✅ Java Solution

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                slow = head;
                while (slow != fast) { slow = slow.next; fast = fast.next; }
                return slow;
            }
        }

        return null;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
