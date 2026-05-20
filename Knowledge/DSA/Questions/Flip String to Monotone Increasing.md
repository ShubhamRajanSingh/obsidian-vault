---
tags:
  - dsa
  - dynamic-programming
  - greedy
  - strings
  - leetcode
leetcode_id: 926
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Flip String to Monotone Increasing

## Problem Link
- https://leetcode.com/problems/flip-string-to-monotone-increasing/

## Intuition
A monotone increasing string has all 0s before 1s.
Track how many flips are needed while traversing.

## Approach
1. Count number of ones seen.
2. If current character is 0:
   - Either flip current 0 to 1
   - Or flip all previous 1s to 0
3. Take minimum.

## Java Solution
```java
class Solution {
    public int minFlipsMonoIncr(String s) {
        int ones = 0;
        int flips = 0;

        for (char ch : s.toCharArray()) {
            if (ch == '1') {
                ones++;
            } else {
                flips = Math.min(flips + 1, ones);
            }
        }

        return flips;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Greedy + DP hybrid pattern.
- Prefix tracking optimization.
