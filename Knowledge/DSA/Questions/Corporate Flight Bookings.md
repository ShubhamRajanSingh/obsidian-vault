---
tags:
  - dsa
  - prefix-sum
  - difference-array
  - arrays
  - leetcode
leetcode_id: 1109
complexity:
  time: O(n + bookings)
  space: O(n)
status: solved
language: java
---

# Corporate Flight Bookings

## Problem Link
- https://leetcode.com/problems/corporate-flight-bookings/

## Intuition
Range increment updates can be optimized using a difference array.

## Approach
1. Apply:
   - +seats at left boundary
   - -seats after right boundary
2. Compute prefix sums.
3. Recover final seat counts.

## Java Solution
```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings,
                                    int n) {

        int[] diff = new int[n];

        for (int[] booking : bookings) {
            int left = booking[0] - 1;
            int right = booking[1] - 1;
            int seats = booking[2];

            diff[left] += seats;

            if (right + 1 < n) {
                diff[right + 1] -= seats;
            }
        }

        for (int i = 1; i < n; i++) {
            diff[i] += diff[i - 1];
        }

        return diff;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n + bookings)
- Space Complexity: O(n)

## Key Takeaways
- Difference array optimization.
- Efficient range updates.
