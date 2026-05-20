---
tags:
  - dsa
  - dynamic-programming
  - game-theory
  - recursion
  - leetcode
leetcode_id: 877
complexity:
  time: O(n^2)
  space: O(n^2)
status: solved
language: java
---

# Stone Game

## Problem Link
- https://leetcode.com/problems/stone-game/

## Intuition
Players choose optimally.
DP stores maximum score difference current player can achieve.

## Approach
1. Define dp[i][j]:
   maximum score difference.
2. Choices:
   - take left pile
   - take right pile
3. Return whether difference > 0.

## Java Solution
```java
class Solution {
    public boolean stoneGame(int[] piles) {
        int n = piles.length;

        int[][] dp = new int[n][n];

        for (int i = 0; i < n; i++) {
            dp[i][i] = piles[i];
        }

        for (int len = 2; len <= n; len++) {
            for (int i = 0; i + len - 1 < n; i++) {
                int j = i + len - 1;

                dp[i][j] = Math.max(
                    piles[i] - dp[i + 1][j],
                    piles[j] - dp[i][j - 1]
                );
            }
        }

        return dp[0][n - 1] > 0;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^2)
- Space Complexity: O(n^2)

## Key Takeaways
- Minimax DP.
- Score difference formulation.
