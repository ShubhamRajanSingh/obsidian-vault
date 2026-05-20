---
tags:
  - dsa
  - dynamic-programming
  - matrix
  - arrays
  - leetcode
leetcode_id: 764
complexity:
  time: O(n^2)
  space: O(n^2)
status: solved
language: java
---

# Largest Plus Sign

## Problem Link
- https://leetcode.com/problems/largest-plus-sign/

## Intuition
For every cell, compute continuous ones in all four directions.
Minimum arm length determines plus size.

## Approach
1. Initialize grid.
2. DP from:
   - left
   - right
   - top
   - bottom
3. Compute minimum directional value.
4. Track maximum.

## Java Solution
```java
class Solution {
    public int orderOfLargestPlusSign(int n, int[][] mines) {
        int[][] dp = new int[n][n];

        for (int[] row : dp) {
            Arrays.fill(row, n);
        }

        Set<Integer> banned = new HashSet<>();

        for (int[] mine : mines) {
            banned.add(mine[0] * n + mine[1]);
        }

        for (int i = 0; i < n; i++) {
            int count = 0;

            for (int j = 0; j < n; j++) {
                count = banned.contains(i * n + j) ? 0 : count + 1;
                dp[i][j] = Math.min(dp[i][j], count);
            }

            count = 0;

            for (int j = n - 1; j >= 0; j--) {
                count = banned.contains(i * n + j) ? 0 : count + 1;
                dp[i][j] = Math.min(dp[i][j], count);
            }
        }

        int answer = 0;

        for (int j = 0; j < n; j++) {
            int count = 0;

            for (int i = 0; i < n; i++) {
                count = banned.contains(i * n + j) ? 0 : count + 1;
                dp[i][j] = Math.min(dp[i][j], count);
            }

            count = 0;

            for (int i = n - 1; i >= 0; i--) {
                count = banned.contains(i * n + j) ? 0 : count + 1;
                dp[i][j] = Math.min(dp[i][j], count);
                answer = Math.max(answer, dp[i][j]);
            }
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^2)
- Space Complexity: O(n^2)

## Key Takeaways
- Multi-direction DP.
- Matrix preprocessing technique.
