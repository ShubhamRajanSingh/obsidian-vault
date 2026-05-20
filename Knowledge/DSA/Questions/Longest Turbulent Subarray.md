---
tags:
  - dsa
  - sliding-window
  - arrays
  - dynamic-programming
  - leetcode
leetcode_id: 978
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Longest Turbulent Subarray

## Problem Link
- https://leetcode.com/problems/longest-turbulent-subarray/

## Intuition
A turbulent subarray alternates between greater-than and less-than comparisons.
Track current alternating sequence length.

## Approach
1. Compare adjacent elements.
2. Maintain current turbulent length.
3. Reset when equality appears.
4. Extend when signs alternate.

## Java Solution
```java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int n = arr.length;
        int answer = 1;
        int left = 0;

        for (int right = 1; right < n; right++) {
            int cmp = Integer.compare(arr[right - 1], arr[right]);

            if (cmp == 0) {
                left = right;
            } else if (right == n - 1 ||
                    cmp * Integer.compare(arr[right], arr[right + 1]) != -1) {
                answer = Math.max(answer, right - left + 1);
                left = right;
            }
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Alternating pattern detection.
- Sliding window comparison technique.
