# Coin Change (Minimum Coins)

## Tags
#DSA #DynamicProgramming #UnboundedKnapsack #InterviewPrep

## Links
[[Unbounded Knapsack]]

---

## 🧠 Problem
Given coins of different denominations and a target amount, find the **minimum number of coins** required to make the amount.

---

# 1️⃣ Pure Recursion (Brute Force)

## Idea
Try all coins at every step.

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int ans = solve(coins, amount);
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    private int solve(int[] coins, int amount) {
        if (amount == 0) return 0;
        if (amount < 0) return Integer.MAX_VALUE;

        int min = Integer.MAX_VALUE;

        for (int coin : coins) {
            int res = solve(coins, amount - coin);

            if (res != Integer.MAX_VALUE) {
                min = Math.min(min, 1 + res);
            }
        }

        return min;
    }
}
```

### Complexity
- Time: Exponential ❌

---

# 2️⃣ Recursion + Memoization (Top-Down DP)

## Idea
Store results to avoid recomputation.

```java
import java.util.*;

class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] memo = new int[amount + 1];
        Arrays.fill(memo, -1);

        int ans = solve(coins, amount, memo);
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    private int solve(int[] coins, int amount, int[] memo) {
        if (amount == 0) return 0;
        if (amount < 0) return Integer.MAX_VALUE;

        if (memo[amount] != -1) return memo[amount];

        int min = Integer.MAX_VALUE;

        for (int coin : coins) {
            int res = solve(coins, amount - coin, memo);

            if (res != Integer.MAX_VALUE) {
                min = Math.min(min, 1 + res);
            }
        }

        return memo[amount] = min;
    }
}
```

### Complexity
- Time: O(n × amount) ✅
- Space: O(amount)

---

# 3️⃣ Bottom-Up DP (Tabulation)

## Idea
Build solution from smaller amounts to larger amounts.

```java
import java.util.*;

class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);

        dp[0] = 0;

        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

### Complexity
- Time: O(n × amount) ✅
- Space: O(amount)

---

## 📌 Notes
- Greedy approach may fail
- Classic **Unbounded Knapsack** problem
- Always explain progression: Recursion → Memoization → DP in interviews
