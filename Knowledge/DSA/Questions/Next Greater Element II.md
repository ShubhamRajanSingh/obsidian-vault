---
tags:
  - dsa
  - monotonic-stack
  - circular-array
  - arrays
  - leetcode
leetcode_id: 503
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Next Greater Element II

## Problem Link
- https://leetcode.com/problems/next-greater-element-ii/

## Intuition
Use monotonic decreasing stack.
Traverse array twice to simulate circular behavior.

## Approach
1. Traverse indices from 2n-1 to 0.
2. Maintain decreasing stack.
3. Remove smaller/equal elements.
4. Stack top becomes next greater element.

## Java Solution
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;

        int[] answer = new int[n];
        Arrays.fill(answer, -1);

        Stack<Integer> stack = new Stack<>();

        for (int i = 2 * n - 1; i >= 0; i--) {
            int index = i % n;

            while (!stack.isEmpty() &&
                   stack.peek() <= nums[index]) {

                stack.pop();
            }

            if (i < n && !stack.isEmpty()) {
                answer[index] = stack.peek();
            }

            stack.push(nums[index]);
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Circular array simulation.
- Monotonic decreasing stack.
