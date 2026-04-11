# Clone Linked List with Random Pointer

## Tags
#DSA #LinkedList #InterviewPrep

## Approach (O(n), O(1) space)
- Interleave nodes
- Assign random
- Separate lists

```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;

        Node curr = head;

        while (curr != null) {
            Node next = curr.next;
            curr.next = new Node(curr.val);
            curr.next.next = next;
            curr = next;
        }

        curr = head;
        while (curr != null) {
            if (curr.random != null)
                curr.next.random = curr.random.next;
            curr = curr.next.next;
        }

        Node dummy = new Node(0);
        Node copy = dummy;
        curr = head;

        while (curr != null) {
            copy.next = curr.next;
            curr.next = curr.next.next;

            copy = copy.next;
            curr = curr.next;
        }

        return dummy.next;
    }
}
```