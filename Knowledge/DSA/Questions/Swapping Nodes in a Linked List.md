# Swapping Nodes in a Linked List

```java
class Solution {
    public ListNode swapNodes(ListNode head,int k){
        ListNode first=head,second=head,temp=head;

        for(int i=1;i<k;i++) first=first.next;

        temp=first;

        while(temp.next!=null){
            second=second.next;
            temp=temp.next;
        }

        int val=first.val;
        first.val=second.val;
        second.val=val;

        return head;
    }
}
```