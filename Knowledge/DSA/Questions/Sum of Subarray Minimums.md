---
tags:
  - dsa
  - monotonic-stack
  - arrays
  - dynamic-programming
  - leetcode
leetcode_id: 907
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Sum of Subarray Minimums

## Problem Link
- https://leetcode.com/problems/sum-of-subarray-minimums/

## Intuition
Each element contributes as the minimum in several subarrays.
Use monotonic stack to count contribution ranges.

## Approach
1. Find previous smaller element.
2. Find next smaller element.
3. Compute contribution:
   arr[i] * leftChoices * rightChoices
4. Sum contributions.

## Java Solution
```java
class Solution {
    public int sumSubarrayMins(int[] arr) {
        int mod = 1_000_000_007;
        int n = arr.length;

        int[] left = new int[n];
        int[] right = new int[n];

        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() &&
                   arr[stack.peek()] > arr[i]) {
                stack.pop();
            }

            left[i] = stack.isEmpty()
                    ? i + 1
                    : i - stack.peek();

            stack.push(i);
        }

        stack.clear();

        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() &&
                   arr[stack.peek()] >= arr[i]) {
                stack.pop();
            }

            right[i] = stack.isEmpty()
                    ? n - i
                    : stack.peek() - i;

            stack.push(i);
        }

        long answer = 0;

        for (int i = 0; i < n; i++) {
            answer += (long) arr[i] * left[i] * right[i];
            answer %= mod;
        }

        return (int) answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Contribution technique.
- Monotonic increasing stack.
