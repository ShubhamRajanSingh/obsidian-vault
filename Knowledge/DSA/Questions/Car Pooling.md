---
tags:
  - dsa
  - prefix-sum
  - greedy
  - arrays
  - leetcode
leetcode_id: 1094
complexity:
  time: O(n + 1000)
  space: O(1001)
status: solved
language: java
---

# Car Pooling

## Problem Link
- https://leetcode.com/problems/car-pooling/

## Intuition
Use difference array to track passenger changes at every location.

## Approach
1. Add passengers at pickup point.
2. Remove passengers at drop point.
3. Compute prefix sum.
4. Ensure capacity is never exceeded.

## Java Solution
```java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int[] diff = new int[1001];

        for (int[] trip : trips) {
            diff[trip[1]] += trip[0];
            diff[trip[2]] -= trip[0];
        }

        int passengers = 0;

        for (int val : diff) {
            passengers += val;

            if (passengers > capacity) {
                return false;
            }
        }

        return true;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1000)

## Key Takeaways
- Difference array / sweep line pattern.
- Efficient range updates.
