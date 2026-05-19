# 518. Coin Change II

**Difficulty:** Medium **Topics:** Array, Dynamic Programming, Unbounded Knapsack **Link:** https://leetcode.com/problems/coin-change-ii/

---

## Problem Statement

You are given an integer `amount` and an array of integers `coins` representing coins of different denominations.

Return the **number of combinations** that make up that amount. If no combination can make up the amount, return `0`.

You may use each coin an **unlimited number of times** (unbounded). The order of coins **does not matter** — `[1,2]` and `[2,1]` are the same combination.

```
amount = 5
coins  = [1, 2, 5]

Output: 4

Combinations:
  5 = 5
  5 = 2 + 2 + 1
  5 = 2 + 1 + 1 + 1
  5 = 1 + 1 + 1 + 1 + 1
```

---

## Examples

### Example 1

```
Input:  amount = 5, coins = [1, 2, 5]
Output: 4
```

### Example 2

```
Input:  amount = 3, coins = [2]
Output: 0   (cannot make 3 with only 2s)
```

### Example 3

```
Input:  amount = 10, coins = [10]
Output: 1
```

---

## Constraints

- `1 <= coins.length <= 300`
- `1 <= coins[i] <= 5000`
- All values in `coins` are **distinct**
- `0 <= amount <= 5000`

---

## Intuition

This is an **Unbounded Knapsack** variant — each coin can be used any number of times.

For each coin `coins[i]` and each amount `w`, we have two choices:

**Choice 1 — Exclude coin `i`:** Don't use this coin at all. Count combinations using only the previous coins.

```
solve(i-1, w)
```

**Choice 2 — Include coin `i`** (only if `coins[i] <= w`): Use this coin once and stay on the same coin (unbounded — can use it again).

```
solve(i, w - coins[i])
```

We **add** both choices because we are **counting** combinations, not maximizing value.

```
solve(i, w) = solve(i-1, w) + solve(i, w - coins[i])
```

### Why `solve(i, ...)` and not `solve(i-1, ...)` for include?

In 0-1 Knapsack, after including item `i` we move to `i-1` (can't reuse). Here, after including coin `i` we stay at `i` (can reuse the same coin).

**Base Cases:**

```
solve(i, 0) = 1   for all i  (amount 0 → exactly 1 way: take nothing)
solve(0, w) = 0   for w > 0  (no coins left, amount not zero → 0 ways)
```

---

## Step 1 — Pure Recursion

```java
class Solution {
    public int change(int amount, int[] coins) {
        return solve(coins, coins.length, amount);
    }

    private int solve(int[] coins, int i, int w) {
        // Base case: amount becomes 0 — one valid combination found
        if (w == 0) return 1;

        // Base case: no coins left but amount > 0 — no valid combination
        if (i == 0) return 0;

        // Coin too large — must exclude
        if (coins[i - 1] > w) {
            return solve(coins, i - 1, w);
        }

        // Exclude coin i  +  Include coin i (stay at i for unbounded reuse)
        return solve(coins, i - 1, w) + solve(coins, i, w - coins[i - 1]);
    }
}
```

### Recursion Tree — `coins=[1,2], amount=3`

```
                    solve(2, 3)
                   /            \
          exclude coin2        include coin2 (coins[1]=2 ≤ 3)
          solve(1, 3)          solve(2, 1)
          /       \            /          \
    excl c1     incl c1   excl c2       incl c2 (2>1, skip)
  solve(0,3)  solve(1,2)  solve(1,1)    —
     = 0      /      \    /      \
          excl c1  incl  excl   incl
        solve(0,2) solve(1,1) solve(0,1) solve(1,0)
           =0      /    \      =0         =1
               excl   incl
             solve(0,1) solve(1,0)
                =0        =1
```

Overlapping subproblem: `solve(1, 1)` appears in multiple branches → **O(2^(n+amount))** without caching.

---

## Step 2 — Memoization (Top-Down DP)

Cache every `(i, w)` pair. Only `n × (amount+1)` unique pairs exist.

```java
class Solution {
    int[][] memo;

    public int change(int amount, int[] coins) {
        int n = coins.length;
        memo = new int[n + 1][amount + 1];
        for (int[] row : memo) Arrays.fill(row, -1); // -1 = not computed
        return solve(coins, n, amount);
    }

    private int solve(int[] coins, int i, int w) {
        // Base cases
        if (w == 0) return 1;
        if (i == 0) return 0;

        // Return cached result
        if (memo[i][w] != -1) return memo[i][w];

        // Coin too large — must exclude
        if (coins[i - 1] > w) {
            return memo[i][w] = solve(coins, i - 1, w);
        }

        // Exclude + Include (unbounded: stay at i)
        return memo[i][w] = solve(coins, i - 1, w)
                           + solve(coins, i, w - coins[i - 1]);
    }
}
```

### What Changes from Recursion?

- Added `memo[n+1][amount+1]` initialized to `-1`
- Added cache check: `if (memo[i][w] != -1) return memo[i][w]`
- Added cache store: `return memo[i][w] = ...`
- **Logic and base cases are identical**

### Complexity After Memoization

||Time|Space|
|---|---|---|
|Pure Recursion|O(2^(n+amount))|O(n + amount) stack|
|Memoization|O(n × amount)|O(n × amount)|

---

## Step 3 — Tabulation (Bottom-Up DP)

`dp[i][w]` = number of combinations using first `i` coins to make amount `w`.

### Recurrence

```
If coins[i-1] > w:
    dp[i][w] = dp[i-1][w]                          ← exclude only

Else:
    dp[i][w] = dp[i-1][w] + dp[i][w - coins[i-1]]  ← exclude + include
                                    ↑
                            dp[i][...] not dp[i-1][...]
                            (same row — unbounded reuse)
```

### Base Cases

```
dp[i][0] = 1  for all i   (amount 0 → 1 way: take nothing)
dp[0][w] = 0  for w > 0   (no coins, amount > 0 → 0 ways)
```

```java
class Solution {
    public int change(int amount, int[] coins) {
        int n = coins.length;
        int[][] dp = new int[n + 1][amount + 1];

        // Base case: 1 way to make amount 0 (take nothing)
        for (int i = 0; i <= n; i++) dp[i][0] = 1;

        // dp[0][w] = 0 for w > 0 (default)

        for (int i = 1; i <= n; i++) {
            for (int w = 1; w <= amount; w++) {
                // Exclude coin i
                dp[i][w] = dp[i - 1][w];

                // Include coin i (if it fits) — use same row for unbounded
                if (coins[i - 1] <= w) {
                    dp[i][w] += dp[i][w - coins[i - 1]];
                }
            }
        }

        return dp[n][amount];
    }
}
```

---

## DP Table Visualized

**`coins = [1, 2, 5]`, `amount = 5`**

```
       w=0  w=1  w=2  w=3  w=4  w=5
i=0  [  1    0    0    0    0    0  ]   no coins
i=1  [  1    1    1    1    1    1  ]   coin=1
i=2  [  1    1    2    2    3    3  ]   coin=2
i=3  [  1    1    2    2    3    4  ]   coin=5

Answer: dp[3][5] = 4  ✅
```

**Reading key cells:**

- `dp[1][3]`: coin=1 ≤ 3 → `dp[0][3] + dp[1][2]` = 0+1 = 1 ✅ (only `1+1+1`)
- `dp[2][2]`: coin=2 ≤ 2 → `dp[1][2] + dp[2][0]` = 1+1 = 2 ✅ (`1+1` or `2`)
- `dp[2][4]`: coin=2 ≤ 4 → `dp[1][4] + dp[2][2]` = 1+2 = 3 ✅ (`1+1+1+1`, `2+1+1`, `2+2`)
- `dp[3][5]`: coin=5 ≤ 5 → `dp[2][5] + dp[3][0]` = 3+1 = 4 ✅

---

## Dry Run — Step by Step

**`coins = [1, 2]`, `amount = 3`**

```
Base: dp[i][0] = 1 for all i

       w=0  w=1  w=2  w=3
i=0  [  1    0    0    0  ]

i=1 (coin=1):
  w=1: dp[0][1] + dp[1][0] = 0+1 = 1
  w=2: dp[0][2] + dp[1][1] = 0+1 = 1
  w=3: dp[0][3] + dp[1][2] = 0+1 = 1
i=1  [  1    1    1    1  ]

i=2 (coin=2):
  w=1: coin>w → dp[1][1] = 1
  w=2: dp[1][2] + dp[2][0] = 1+1 = 2
  w=3: dp[1][3] + dp[2][1] = 1+1 = 2
i=2  [  1    1    2    2  ]

Answer: dp[2][3] = 2  ✅
Combinations: [1,1,1] and [1,2]
```

---

## Step 4 — Space Optimized (1D Array)

Since `dp[i][w]` depends on `dp[i-1][w]` (exclude) and `dp[i][w - coins[i-1]]` (include, same row), compress to 1D.

**Critical:** iterate `w` **left to right** so that `dp[w - coins[i-1]]` is already updated for the current coin — allowing reuse (unbounded).

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1; // base case: 1 way to make amount 0

        for (int coin : coins) {
            // LEFT TO RIGHT: allows reusing the same coin multiple times
            for (int w = coin; w <= amount; w++) {
                dp[w] += dp[w - coin];
            }
        }

        return dp[amount];
    }
}
```

### Why Left to Right? (Opposite of 0-1 Knapsack)

```
0-1 Knapsack  → RIGHT to LEFT → each item used at most once
Unbounded     → LEFT to RIGHT → each coin can be reused

When computing dp[w] left to right:
  dp[w - coin] is already updated in this iteration
  → means the current coin was already counted for w-coin
  → so we can reuse it for w  ✅

If we went right to left:
  dp[w - coin] is from the previous coin's iteration
  → each coin would be used at most once ❌ (that's 0-1 knapsack)
```

---

## 0-1 Knapsack vs Coin Change II (Unbounded)

||0-1 Knapsack|Coin Change II|
|---|---|---|
|Item reuse|At most once|Unlimited|
|Recursion include|`solve(i-1, w-wt[i])`|`solve(i, w-coins[i])`|
|2D tabulation include|`dp[i-1][w-wt[i]]`|`dp[i][w-coins[i]]`|
|1D traversal|Right to left|**Left to right**|
|Combining choices|`max(exclude, include)`|`exclude + include` (counting)|

---

## Progression Summary

|Approach|Time|Space|Notes|
|---|---|---|---|
|Recursion|O(2^(n+amount))|O(n+amount)|Exponential — TLE|
|Memoization|O(n × amount)|O(n × amount)|Top-down|
|Tabulation|O(n × amount)|O(n × amount)|Bottom-up|
|Space Optimized|O(n × amount)|O(amount)|Best; left-to-right traversal|

---

## Common Mistakes to Avoid

1. **Going right to left in the 1D approach** — Right to left prevents coin reuse, converting this into a 0-1 knapsack that counts combinations using each coin at most once. Always go left to right for unbounded.
    
2. **Using `dp[i-1][w - coins[i-1]]` instead of `dp[i][w - coins[i-1]]`** in 2D tabulation — The previous row means the coin is used at most once. Use the same row (`dp[i]`) for unbounded.
    
3. **Forgetting `dp[0] = 1`** — The base case "1 way to make amount 0" is essential. Without it, all values stay 0 and the answer is always 0.
    
4. **Confusing combinations with permutations** — This problem counts combinations (`[1,2]` == `[2,1]`). If it asked for permutations (order matters), the DP structure would be different — iterate over amounts in the outer loop and coins in the inner loop.
    
5. **Initializing `memo` with `0` instead of `-1`** — `0` is a valid answer (e.g. no combination possible). Use `-1` as the "not computed" sentinel.
    

---

## Combinations vs Permutations

A common follow-up: what if order matters?

```
amount = 3, coins = [1, 2]

Combinations (this problem): 2   → [1,1,1], [1,2]
Permutations (order matters): 3  → [1,1,1], [1,2], [2,1]
```

For permutations, swap the loop order:

```java
// Permutations — order matters
for (int w = 1; w <= amount; w++) {        // outer: amount
    for (int coin : coins) {               // inner: coins
        if (coin <= w) dp[w] += dp[w - coin];
    }
}
```

For combinations (this problem):

```java
// Combinations — order doesn't matter
for (int coin : coins) {                   // outer: coins
    for (int w = coin; w <= amount; w++) { // inner: amount
        dp[w] += dp[w - coin];
    }
}
```

The outer loop being **coins** ensures each coin group is considered once, preventing permutation counting.

---

## Related Problems

|#|Problem|Relationship|
|---|---|---|
|LC 518|**Coin Change II** ← You are here|Unbounded knapsack — count combinations|
|LC 322|Coin Change|Unbounded knapsack — minimize coins used|
|LC 416|Partition Equal Subset Sum|0-1 Knapsack — can we reach W/2|
|LC 494|Target Sum|0-1 Knapsack — count subsets with target sum|
|LC 377|Combination Sum IV|Unbounded — count permutations (order matters)|
|LC 279|Perfect Squares|Unbounded knapsack — minimize count|