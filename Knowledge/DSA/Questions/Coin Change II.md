# Coin Change II (Number of Ways)

## Tags
#DSA #DynamicProgramming #UnboundedKnapsack #InterviewPrep

## Links
[[Unbounded Knapsack]]

---

## Problem
Given coins of different denominations and a target amount, find the **number of ways** to make that amount.

---

## Approach (Dynamic Programming - O(n × amount))
- Use 1D DP array
- dp[i] = number of ways to make amount i
- Initialize dp[0] = 1 (one way to make 0)
- Iterate coins first (to avoid duplicates)

---

## Java Solution (Optimal)

```java
import java.util.*;

class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;

        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] += dp[i - coin];
            }
        }

        return dp[amount];
    }
}
```

---

## Complexity
- Time: O(n × amount)
- Space: O(amount)

---

## Notes
- Order does NOT matter (combinations, not permutations)
- Must loop coins first to avoid duplicate counting
- Classic variation of [[Unbounded Knapsack]]
