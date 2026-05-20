# Convert Binary Number in a Linked List to Integer

```java
class Solution {
    public int getDecimalValue(ListNode head){
        int ans=0;

        while(head!=null){
            ans=ans*2+head.val;
            head=head.next;
        }

        return ans;
    }
}
```