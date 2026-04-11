# Core DSA Questions

## Tags
#DSA #InterviewPrep #Greedy #Arrays #LinkedList #TopQuestions

---

## 🔁 Rotate a Linked List

### Approach (Optimal - O(n))
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

---

## 🧬 Clone Linked List with Random Pointer

### Approach (O(n), O(1) space)
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

---

## 🔢 3Sum

### Approach (Two Pointer)

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int l = i + 1, r = nums.length - 1;

            while (l < r) {
                int sum = nums[i] + nums[l] + nums[r];

                if (sum == 0) {
                    res.add(Arrays.asList(nums[i], nums[l], nums[r]));

                    while (l < r && nums[l] == nums[l + 1]) l++;
                    while (l < r && nums[r] == nums[r - 1]) r--;

                    l++;
                    r--;
                } else if (sum < 0) l++;
                else r--;
            }
        }

        return res;
    }
}
```

---

## 🌧 Trapping Rainwater

### Approach (Two Pointer - O(n))

```java
class Solution {
    public int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int leftMax = 0, rightMax = 0, water = 0;

        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) leftMax = height[left];
                else water += leftMax - height[left];
                left++;
            } else {
                if (height[right] >= rightMax) rightMax = height[right];
                else water += rightMax - height[right];
                right--;
            }
        }

        return water;
    }
}
```

---

## 🚉 Minimum Platforms Required

### Approach (Greedy + Sorting)

```java
import java.util.*;

class Solution {
    static int findPlatform(int arr[], int dep[]) {
        Arrays.sort(arr);
        Arrays.sort(dep);

        int i = 0, j = 0, platforms = 0, max = 0;

        while (i < arr.length && j < dep.length) {
            if (arr[i] <= dep[j]) {
                platforms++;
                max = Math.max(max, platforms);
                i++;
            } else {
                platforms--;
                j++;
            }
        }

        return max;
    }
}
```

---

## 🏢 N Meetings in One Room

### Approach (Greedy - Finish Time)

```java
import java.util.*;

class Meeting {
    int start, end;
    Meeting(int s, int e) {
        start = s;
        end = e;
    }
}

class Solution {
    public int maxMeetings(int start[], int end[]) {
        int n = start.length;
        Meeting[] meetings = new Meeting[n];

        for (int i = 0; i < n; i++)
            meetings[i] = new Meeting(start[i], end[i]);

        Arrays.sort(meetings, (a, b) -> a.end - b.end);

        int count = 1;
        int lastEnd = meetings[0].end;

        for (int i = 1; i < n; i++) {
            if (meetings[i].start > lastEnd) {
                count++;
                lastEnd = meetings[i].end;
            }
        }

        return count;
    }
}
```

---

## 📌 Notes
- All solutions are optimal (no brute force)
- Patterns: Greedy, Two Pointer, Linked List
