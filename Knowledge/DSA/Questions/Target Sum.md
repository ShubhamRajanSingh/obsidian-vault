---
tags:
  - dsa
  - dynamic-programming
  - recursion
  - knapsack
  - leetcode
leetcode_id: 494
complexity:
  time: O(n * sum)
  space: O(sum)
status: solved
language: java
---

# Target Sum

## Problem Link
- https://leetcode.com/problems/target-sum/

## Intuition
Assigning '+' and '-' signs can be transformed into subset sum.

Let:
positive - negative = target
positive + negative = total

Then:
positive = (target + total) / 2

## Approach
1. Compute transformed subset target.
2. Use subset-count DP.
3. Count ways to achieve target sum.

## Java Solution
```java
class Solution {
    public int findTargetSumWays(int[] nums,
                                 int target) {

        int total = 0;

        for (int num : nums) {
            total += num;
        }

        if ((target + total) % 2 != 0 ||
            Math.abs(target) > total) {

            return 0;
        }

        int subset = (target + total) / 2;

        int[] dp = new int[subset + 1];
        dp[0] = 1;

        for (int num : nums) {
            for (int sum = subset;
                 sum >= num;
                 sum--) {

                dp[sum] += dp[sum - num];
            }
        }

        return dp[subset];
    }
}
```

## Complexity Analysis
- Time Complexity: O(n * sum)
- Space Complexity: O(sum)

## Key Takeaways
- Subset sum transformation.
- Counting DP pattern.
