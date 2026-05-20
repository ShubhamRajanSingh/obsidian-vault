---
tags:
  - dsa
  - intervals
  - sorting
  - greedy
  - leetcode
leetcode_id: 1288
complexity:
  time: O(n log n)
  space: O(1)
status: solved
language: java
---

# Remove Covered Intervals

## Problem Link
- https://leetcode.com/problems/remove-covered-intervals/

## Intuition
If intervals are sorted properly, covered intervals can be detected by tracking the farthest ending point seen so far.

## Approach
1. Sort intervals:
   - Start ascending
   - End descending
2. Track maximum end seen.
3. If current interval end is smaller or equal, it is covered.
4. Otherwise increment answer.

## Java Solution
```java
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> {
            if (a[0] == b[0]) {
                return b[1] - a[1];
            }
            return a[0] - b[0];
        });

        int count = 0;
        int maxEnd = 0;

        for (int[] interval : intervals) {
            if (interval[1] > maxEnd) {
                count++;
                maxEnd = interval[1];
            }
        }

        return count;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(1)

## Key Takeaways
- Sorting trick is essential.
- Descending end sort prevents false coverage.
