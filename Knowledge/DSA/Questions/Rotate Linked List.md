# Rotate Linked List

## Tags
#DSA #LinkedList #InterviewPrep

## Approach (Optimal - O(n))
- Find length
- Make circular
- Break at new head

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) return head;

        ListNode temp = head;
        int len = 1;

        while (temp.next != null) {
            temp = temp.next;
            len++;
        }

        temp.next = head;
        k = k % len;
        int steps = len - k;

        while (steps-- > 0) temp = temp.next;

        head = temp.next;
        temp.next = null;

        return head;
    }
}
```