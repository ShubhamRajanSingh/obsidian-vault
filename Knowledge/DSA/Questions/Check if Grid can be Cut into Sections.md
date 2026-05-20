---
tags:
  - dsa
  - geometry
  - intervals
  - greedy
  - leetcode
leetcode_id: 3394
complexity:
  time: O(n log n)
  space: O(n)
status: solved
language: java
---

# Check if Grid can be Cut into Sections

## Problem Link
- https://leetcode.com/problems/check-if-grid-can-be-cut-into-sections/

## Intuition
The problem reduces to checking whether intervals can be separated into at least three disconnected groups.

## Approach
1. Project rectangles onto x-axis and y-axis.
2. Merge overlapping intervals.
3. If merged interval count >= 3 on either axis, answer is true.

## Java Solution
```java
class Solution {
    public boolean checkValidCuts(int n, int[][] rectangles) {
        List<int[]> xIntervals = new ArrayList<>();
        List<int[]> yIntervals = new ArrayList<>();

        for (int[] r : rectangles) {
            xIntervals.add(new int[]{r[0], r[2]});
            yIntervals.add(new int[]{r[1], r[3]});
        }

        return countMerged(xIntervals) >= 3 ||
               countMerged(yIntervals) >= 3;
    }

    private int countMerged(List<int[]> intervals) {
        intervals.sort((a, b) -> a[0] - b[0]);

        int groups = 0;
        int end = -1;

        for (int[] interval : intervals) {
            if (interval[0] >= end) {
                groups++;
                end = interval[1];
            } else {
                end = Math.max(end, interval[1]);
            }
        }

        return groups;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(n)

## Key Takeaways
- Geometry problems often reduce to interval merging.
- Projection technique is powerful.
