---
tags:
  - dsa
  - linked-list
  - hashmap
  - medium
  - leetcode
  - striver
aliases:
  - Copy List with Random Pointer
  - LeetCode 138
difficulty: Medium
topic: Linked List / HashMap
pattern: Two-pass with HashMap or interleaving
leetcode_id: 138
date: 2026-05-19
---

# Copy List with Random Pointer

## 📌 Problem Statement

> A linked list where each node has a `next` and a `random` pointer (which can point to any node or null). Return a deep copy of the list.

**LeetCode #138** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (HashMap)

```java
class Solution {
    public Node copyRandomList(Node head) {
        Map<Node, Node> map = new HashMap<>();
        Node curr = head;
        while (curr != null) { map.put(curr, new Node(curr.val)); curr = curr.next; }
        curr = head;
        while (curr != null) {
            map.get(curr).next = map.get(curr.next);
            map.get(curr).random = map.get(curr.random);
            curr = curr.next;
        }
        return map.get(head);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
