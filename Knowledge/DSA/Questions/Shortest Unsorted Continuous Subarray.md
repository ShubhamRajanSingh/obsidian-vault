---
tags:
  - dsa
  - arrays
  - sorting
  - monotonic-stack
  - leetcode
leetcode_id: 581
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Shortest Unsorted Continuous Subarray

## Problem Link
- https://leetcode.com/problems/shortest-unsorted-continuous-subarray/

## Intuition
The unsorted segment disrupts global ordering.
Track misplaced minimum and maximum values.

## Approach
1. Traverse left to right:
   detect decreasing violations.
2. Traverse right to left:
   detect increasing violations.
3. Expand boundaries appropriately.

## Java Solution
```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int n = nums.length;

        int start = -1;
        int end = -2;

        int max = nums[0];
        int min = nums[n - 1];

        for (int i = 1; i < n; i++) {
            max = Math.max(max, nums[i]);

            if (nums[i] < max) {
                end = i;
            }
        }

        for (int i = n - 2; i >= 0; i--) {
            min = Math.min(min, nums[i]);

            if (nums[i] > min) {
                start = i;
            }
        }

        return end - start + 1;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Boundary expansion logic.
- Order violation detection.
