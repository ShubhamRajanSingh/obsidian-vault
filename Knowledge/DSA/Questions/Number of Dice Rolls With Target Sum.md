---
tags:
  - dsa
  - dynamic-programming
  - recursion
  - combinatorics
  - leetcode
leetcode_id: 1155
complexity:
  time: O(n * target * k)
  space: O(n * target)
status: solved
language: java
---

# Number of Dice Rolls With Target Sum

## Problem Link
- https://leetcode.com/problems/number-of-dice-rolls-with-target-sum/

## Intuition
At every die, try all possible face values.
DP stores number of ways to reach remaining target.

## Approach
1. Define dp[dice][sum].
2. Transition over all face values.
3. Use modulo arithmetic.

## Java Solution
```java
class Solution {
    public int numRollsToTarget(int n,
                                int k,
                                int target) {

        int mod = 1_000_000_007;

        int[][] dp = new int[n + 1][target + 1];
        dp[0][0] = 1;

        for (int dice = 1; dice <= n; dice++) {
            for (int sum = 1; sum <= target; sum++) {
                for (int face = 1;
                     face <= k && face <= sum;
                     face++) {

                    dp[dice][sum] =
                        (dp[dice][sum] +
                        dp[dice - 1][sum - face]) % mod;
                }
            }
        }

        return dp[n][target];
    }
}
```

## Complexity Analysis
- Time Complexity: O(n * target * k)
- Space Complexity: O(n * target)

## Key Takeaways
- Classic counting DP.
- Multi-choice transitions.
