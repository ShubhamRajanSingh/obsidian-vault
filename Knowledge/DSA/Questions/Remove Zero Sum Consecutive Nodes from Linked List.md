---
tags:
  - dsa
  - linked-list
  - prefix-sum
  - hashmap
  - leetcode
leetcode_id: 1171
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Remove Zero Sum Consecutive Nodes from Linked List

## Problem Link
- https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/

## Intuition
If same prefix sum appears twice, nodes in between sum to zero.

## Approach
1. Use dummy node.
2. Store latest node for every prefix sum.
3. Second pass reconnects pointers.

## Java Solution
```java
class Solution {
    public ListNode removeZeroSumSublists(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        Map<Integer, ListNode> map = new HashMap<>();

        int prefix = 0;

        for (ListNode node = dummy;
             node != null;
             node = node.next) {

            prefix += node.val;
            map.put(prefix, node);
        }

        prefix = 0;

        for (ListNode node = dummy;
             node != null;
             node = node.next) {

            prefix += node.val;
            node.next = map.get(prefix).next;
        }

        return dummy.next;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Prefix sum cancellation.
- Linked list reconstruction.
