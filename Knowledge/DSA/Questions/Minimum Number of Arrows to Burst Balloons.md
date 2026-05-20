---
tags:
  - dsa
  - greedy
  - intervals
  - sorting
  - leetcode
leetcode_id: 452
complexity:
  time: O(n log n)
  space: O(1)
status: solved
language: java
---

# Minimum Number of Arrows to Burst Balloons

## Problem Link
- https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/

## Intuition
Overlapping balloons can be burst using one arrow.
Greedily shoot arrows at the end of intervals.

## Approach
1. Sort balloons by ending coordinate.
2. Shoot arrow at first balloon end.
3. Skip all overlapping balloons.
4. Count arrows.

## Java Solution
```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points,
            (a, b) -> Integer.compare(a[1], b[1]));

        int arrows = 1;
        int end = points[0][1];

        for (int[] point : points) {
            if (point[0] > end) {
                arrows++;
                end = point[1];
            }
        }

        return arrows;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(1)

## Key Takeaways
- Interval greedy optimization.
- Sort by ending time pattern.
