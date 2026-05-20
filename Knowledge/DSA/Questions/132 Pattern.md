---
tags:
  - dsa
  - monotonic-stack
  - arrays
  - greedy
  - leetcode
leetcode_id: 456
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# 132 Pattern

## Problem Link
- https://leetcode.com/problems/132-pattern/

## Intuition
Traverse from right to left.
Maintain possible middle element using a monotonic stack.

## Approach
1. Traverse array backward.
2. Maintain decreasing stack.
3. Track valid '2' candidate.
4. Detect:
   nums[i] < middleValue.

## Java Solution
```java
class Solution {
    public boolean find132pattern(int[] nums) {
        Stack<Integer> stack = new Stack<>();

        int middle = Integer.MIN_VALUE;

        for (int i = nums.length - 1; i >= 0; i--) {
            if (nums[i] < middle) {
                return true;
            }

            while (!stack.isEmpty() &&
                   nums[i] > stack.peek()) {

                middle = stack.pop();
            }

            stack.push(nums[i]);
        }

        return false;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Reverse traversal optimization.
- Monotonic stack pattern detection.
