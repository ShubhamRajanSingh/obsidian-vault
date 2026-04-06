# 🔁 Rotate Linked List (Right by k)

---

## 🧠 Problem
Given the head of a singly linked list, rotate the list to the right by k places.

Example:
Input: 1 → 2 → 3 → 4 → 5, k = 2  
Output: 4 → 5 → 1 → 2 → 3

---

## 🧪 Approach 1: Value Manipulation (My Approach)

### 💡 Idea
- Compute length
- Take first k nodes and store in a new list
- Swap values across nodes to simulate rotation

### ⚙️ Code
```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if(head == null || head.next == null || k == 0){
            return head;
        }
        int length = 0;
        ListNode wptr = head;
        while(wptr != null){
            length++;
            wptr=wptr.next;
        }
        int newK = k % length;
        if(newK == 0){
            return head;
        }

        ListNode newHead = null, wptr2= null;
        wptr = head;

        int counter = 0;
        while(counter < newK){
            ListNode node = new ListNode(wptr.val);
            if(newHead == null){
                newHead = node;
                wptr2 = node;
            }else{
                wptr2.next = node;
                wptr2 = node;
            }
            wptr = wptr.next;
            counter++;
        }
        wptr2 = newHead;

        while(wptr != null){
            int temp = wptr.val;
            wptr.val = wptr2.val;
            wptr2.val = temp;

            wptr = wptr.next;
            wptr2 = wptr2.next;
            if(wptr2 == null){
                wptr2 = newHead;
            }
        }
        wptr = head;
        counter = 0;
        while(counter < newK){
            wptr.val = wptr2.val;
            wptr = wptr.next;
            wptr2 = wptr2.next;
            if(wptr2 == null){
                wptr2 = newHead;
            }
            counter++;
        }
        return head;
    }
}
```

### ⚠️ Issues
- Uses extra space O(k)
- Swaps values instead of nodes
- Complex and hard to maintain
- Not ideal for interviews

---

## 🚀 Approach 2: Optimal (Pointer Manipulation)

### 💡 Idea
- Find length
- Make list circular
- Break at correct position

### ⚙️ Code
```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;

        ListNode curr = head;
        int length = 1;
        while (curr.next != null) {
            curr = curr.next;
            length++;
        }

        curr.next = head; // make circular

        k = k % length;
        int steps = length - k;

        ListNode newTail = head;
        for (int i = 1; i < steps; i++) {
            newTail = newTail.next;
        }

        ListNode newHead = newTail.next;
        newTail.next = null;

        return newHead;
    }
}
```

### ✅ Advantages
- O(n) time
- O(1) space
- Clean and interview-friendly

---

## 🧩 Key Takeaways
- Prefer pointer manipulation over value swapping in linked lists
- Use circular list trick for rotation problems
- Always reduce k using modulo

---

## 🔥 Tags
#linkedlist #rotation #dsa #questions