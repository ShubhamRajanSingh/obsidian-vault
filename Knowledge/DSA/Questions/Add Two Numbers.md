---
tags:
  - dsa
  - linked-list
  - math
  - medium
  - leetcode
aliases:
  - Add Two Numbers
  - LeetCode 2
difficulty: Medium
topic: Linked List / Math
pattern: Dummy Head + Carry
leetcode_id: 2
date: 2026-05-19
---

# Add Two Numbers

## 📌 Problem Statement

> You are given two non-empty linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each node contains a single digit. Add the two numbers and return the sum as a linked list.

**LeetCode #2** | Difficulty: 🟡 Medium

### Example
```
Input:  l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807
```

---

## 🧠 Intuition

Simulate grade-school addition digit by digit from the least significant digit (head of list). Track carry as you go.

---

## ✅ Java Solution

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        int carry = 0;

        while (l1 != null || l2 != null || carry != 0) {
            int sum = carry;
            if (l1 != null) { sum += l1.val; l1 = l1.next; }
            if (l2 != null) { sum += l2.val; l2 = l2.next; }
            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
        }

        return dummy.next;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(max(m, n)) |
| Space | O(max(m, n)) |

---

## 🔗 Related

- [[Clone Linked List with Random Pointer]]
- [[Rotate Linked List]]
