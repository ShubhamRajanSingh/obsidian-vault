# 887. Super Egg Drop
#REVISIT 

**Difficulty:** Hard **Topics:** Dynamic Programming, Binary Search, Math **Link:** https://leetcode.com/problems/super-egg-drop/

---

## Problem Statement

You are given `k` eggs and a building with `n` floors. You want to know the **minimum number of moves** (trials) to determine the **critical floor** `f` — the highest floor from which an egg dropped will **not break**.

Rules:

- If an egg is dropped from floor `x` and it **breaks** → `f < x` (critical floor is below `x`)
- If an egg is dropped from floor `x` and it **doesn't break** → `f >= x` (critical floor is at or above `x`)
- A broken egg **cannot be reused**
- An unbroken egg **can be reused**
- `f` can be any floor from `0` to `n` (if no egg breaks even at floor `n`, then `f = n`)

Return the **minimum number of moves** in the **worst case** to determine `f` with certainty.

```
k = 1, n = 2   →  Output: 2
k = 2, n = 6   →  Output: 3
k = 3, n = 14  →  Output: 4
```

---

## Examples

### Example 1

```
Input:  k=1, n=2
Output: 2

With 1 egg, must try floor 1, then floor 2 sequentially.
Worst case = 2 moves.
```

### Example 2

```
Input:  k=2, n=6
Output: 3

Strategy: drop from floors 3, 5, 6
  Try floor 3: breaks → try 1,2 (1 egg left, 2 floors → 2 moves)
              doesn't break → try floor 5
  Try floor 5: breaks → try 4 (1 egg left, 1 floor → 1 move)
              doesn't break → try floor 6
  Worst case = 3 moves.
```

### Example 3

```
Input:  k=3, n=14
Output: 4
```

---

## Constraints

- `1 <= k <= 100`
- `1 <= n <= 10^4`

---

## Intuition

### The Core Decision

At every step, choose a floor `x` to drop an egg from:

**If egg breaks** → critical floor is below `x`

- We have `k-1` eggs, need to check `x-1` floors below
- Subproblem: `solve(k-1, x-1)`

**If egg doesn't break** → critical floor is at or above `x`

- We still have `k` eggs, need to check `n-x` floors above
- Subproblem: `solve(k, n-x)`

We want the **worst case** of both outcomes (we don't control which happens):

```
cost(x) = 1 + max(solve(k-1, x-1), solve(k, n-x))
```

We choose `x` to **minimize** this worst case:

```
solve(k, n) = min over all x from 1 to n of:
              1 + max(solve(k-1, x-1), solve(k, n-x))
```

**Base Cases:**

```
solve(k, 0) = 0    (0 floors → 0 trials needed)
solve(k, 1) = 1    (1 floor → 1 trial needed)
solve(1, n) = n    (1 egg → must try every floor sequentially)
```

---

## Step 1 — Pure Recursion

```java
class Solution {
    public int superEggDrop(int k, int n) {
        return solve(k, n);
    }

    private int solve(int k, int n) {
        // Base cases
        if (n == 0) return 0;
        if (n == 1) return 1;
        if (k == 1) return n; // must check every floor one by one

        int min = Integer.MAX_VALUE;

        // Try dropping from every floor x
        for (int x = 1; x <= n; x++) {
            int worst = 1 + Math.max(
                solve(k - 1, x - 1), // egg breaks
                solve(k, n - x)      // egg doesn't break
            );
            min = Math.min(min, worst);
        }

        return min;
    }
}
```

**Time Complexity: O(k × n²)** — for each `(k, n)` we try all `n` floors. Without memoization, states repeat → exponential blowup.

---

## Step 2 — Memoization (Top-Down DP)

Cache every `(k, n)` pair. Only `k × n` unique states exist.

```java
class Solution {
    int[][] memo;

    public int superEggDrop(int k, int n) {
        memo = new int[k + 1][n + 1];
        for (int[] row : memo) Arrays.fill(row, -1);
        return solve(k, n);
    }

    private int solve(int k, int n) {
        // Base cases
        if (n == 0) return 0;
        if (n == 1) return 1;
        if (k == 1) return n;

        // Return cached
        if (memo[k][n] != -1) return memo[k][n];

        int min = Integer.MAX_VALUE;

        for (int x = 1; x <= n; x++) {
            int worst = 1 + Math.max(
                solve(k - 1, x - 1),
                solve(k, n - x)
            );
            min = Math.min(min, worst);
        }

        return memo[k][n] = min;
    }
}
```

**Time: O(k × n²)** — still slow for large `n`. Each state tries `n` floors linearly.

---

## Step 3 — Memoization + Binary Search Optimization

### Key Observation

For a fixed `k` and `n`, as floor `x` increases:

- `solve(k-1, x-1)` → **increases** (more floors below)
- `solve(k, n-x)` → **decreases** (fewer floors above)

Their `max` forms a **valley shape** — first decreasing then increasing.

```
         solve(k, n-x)  ↘
                          ↘   ← minimum of max is here (valley)
                            ↗
                              solve(k-1, x-1)  ↗
```

The optimal `x` is where the two functions **cross** — we can binary search for it!

```java
class Solution {
    int[][] memo;

    public int superEggDrop(int k, int n) {
        memo = new int[k + 1][n + 1];
        for (int[] row : memo) Arrays.fill(row, -1);
        return solve(k, n);
    }

    private int solve(int k, int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        if (k == 1) return n;

        if (memo[k][n] != -1) return memo[k][n];

        int min = Integer.MAX_VALUE;

        // Binary search for the optimal floor x
        int lo = 1, hi = n;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            int breakCase    = solve(k - 1, mid - 1); // egg breaks
            int noBreakCase  = solve(k, n - mid);     // egg doesn't break

            int worst = 1 + Math.max(breakCase, noBreakCase);
            min = Math.min(min, worst);

            // Move toward the crossing point
            if (breakCase < noBreakCase) {
                lo = mid + 1; // break is smaller → go up to balance
            } else {
                hi = mid - 1; // no-break is smaller → go down to balance
            }
        }

        return memo[k][n] = min;
    }
}
```

**Time: O(k × n × log n)** — binary search reduces inner loop from O(n) to O(log n).

---

## Step 4 — Tabulation (Bottom-Up DP)

`dp[i][j]` = minimum moves needed with `i` eggs and `j` floors.

```java
class Solution {
    public int superEggDrop(int k, int n) {
        int[][] dp = new int[k + 1][n + 1];

        // Base cases
        for (int j = 0; j <= n; j++) dp[1][j] = j; // 1 egg → j moves for j floors
        for (int i = 1; i <= k; i++) dp[i][0] = 0; // 0 floors → 0 moves
        for (int i = 1; i <= k; i++) dp[i][1] = 1; // 1 floor → 1 move

        // Fill remaining
        for (int i = 2; i <= k; i++) {
            for (int j = 2; j <= n; j++) {
                dp[i][j] = Integer.MAX_VALUE;

                int lo = 1, hi = j;
                while (lo <= hi) {
                    int mid = lo + (hi - lo) / 2;
                    int breakCase   = dp[i - 1][mid - 1];
                    int noBreakCase = dp[i][j - mid];

                    int worst = 1 + Math.max(breakCase, noBreakCase);
                    dp[i][j] = Math.min(dp[i][j], worst);

                    if (breakCase < noBreakCase) lo = mid + 1;
                    else hi = mid - 1;
                }
            }
        }

        return dp[k][n];
    }
}
```

**Time: O(k × n × log n)**, **Space: O(k × n)**

---

## DP Table Visualized

**`k=2, n=6`**

```
       j=0  j=1  j=2  j=3  j=4  j=5  j=6
i=1  [  0    1    2    3    4    5    6  ]   1 egg: check every floor
i=2  [  0    1    2    2    3    3    3  ]   2 eggs: binary-search-like strategy

Answer: dp[2][6] = 3  ✅
```

**How `dp[2][3] = 2`:**

```
Try x=2:
  break    → dp[1][1] = 1
  no break → dp[2][1] = 1
  worst = 1 + max(1,1) = 2  ← minimum at x=2

So with 2 eggs and 3 floors → 2 trials in worst case ✅
```

---

## Step 5 — Rethinking the Problem (Optimal O(kn) Approach)

### Flip the Question

Instead of asking:

> "Given `k` eggs and `n` floors, what is the minimum number of moves?"

Ask:

> "Given `k` eggs and `t` moves, what is the **maximum number of floors** we can check?"

Let `f(t, k)` = maximum floors we can determine with `t` trials and `k` eggs.

### Recurrence

With `t` trials and `k` eggs, drop from some floor:

- **Egg breaks** → `t-1` trials, `k-1` eggs → can verify `f(t-1, k-1)` floors below
- **Egg doesn't break** → `t-1` trials, `k` eggs → can verify `f(t-1, k)` floors above

Plus the floor we just dropped from:

```
f(t, k) = f(t-1, k-1) + f(t-1, k) + 1
```

**Base Cases:**

```
f(t, 1) = t      (1 egg, t trials → can check t floors linearly)
f(1, k) = 1      (1 trial → can check only 1 floor)
f(0, k) = 0      (0 trials → can check 0 floors)
```

**Answer:** find the minimum `t` such that `f(t, k) >= n`.

```java
class Solution {
    public int superEggDrop(int k, int n) {
        // dp[t][i] = max floors checkable with t trials and i eggs
        // We only need previous row → use 1D array
        int[] dp = new int[k + 1];
        int t = 0;

        while (dp[k] < n) {
            t++;
            // Traverse RIGHT TO LEFT to avoid using updated values
            for (int i = k; i >= 1; i--) {
                dp[i] = dp[i - 1] + dp[i] + 1;
                //       breaks     no-break  +current floor
            }
        }

        return t;
    }
}
```

**Time: O(k × answer)**, **Space: O(k)**

This is the most elegant and efficient solution for this problem.

---

## Dry Run — Flipped Approach

**`k=2, n=6`**

```
dp = [0, 0, 0]   (indices 0,1,2 for 0,1,2 eggs)

t=1:
  i=2: dp[2] = dp[1] + dp[2] + 1 = 0+0+1 = 1
  i=1: dp[1] = dp[0] + dp[1] + 1 = 0+0+1 = 1
  dp = [0, 1, 1]   → max floors with 2 eggs, 1 trial = 1

t=2:
  i=2: dp[2] = dp[1] + dp[2] + 1 = 1+1+1 = 3
  i=1: dp[1] = dp[0] + dp[1] + 1 = 0+1+1 = 2
  dp = [0, 2, 3]   → max floors with 2 eggs, 2 trials = 3

t=3:
  i=2: dp[2] = dp[1] + dp[2] + 1 = 2+3+1 = 6
  i=1: dp[1] = dp[0] + dp[1] + 1 = 0+2+1 = 3
  dp = [0, 3, 6]   → max floors with 2 eggs, 3 trials = 6

dp[2] = 6 >= n=6  →  return t = 3  ✅
```

---

## Why Right to Left in the Flipped Approach?

When updating `dp[i] = dp[i-1] + dp[i] + 1`:

- `dp[i-1]` must be from the **previous trial** (t-1), not the current trial (t)
- Going right to left ensures `dp[i-1]` hasn't been updated yet in this iteration

```
LEFT to RIGHT (wrong):
  i=1: dp[1] updated first (now reflects t trials)
  i=2: dp[2] = dp[1](updated) + dp[2] + 1  ← wrong! dp[1] is already from trial t

RIGHT to LEFT (correct):
  i=2: dp[2] = dp[1](old) + dp[2](old) + 1  ✅
  i=1: dp[1] updated after dp[2] uses it    ✅
```

---

## All Approaches Comparison

|Approach|Time|Space|Notes|
|---|---|---|---|
|Recursion|O(k × n²)|O(k × n) stack|TLE for large n|
|Memoization|O(k × n²)|O(k × n)|TLE for large n|
|Memo + Binary Search|O(k × n log n)|O(k × n)|Passes most cases|
|Tabulation + Binary Search|O(k × n log n)|O(k × n)|Bottom-up|
|**Flipped DP**|**O(k × ans)**|**O(k)**|**Optimal — most elegant**|

---

## Intuition Behind the Flipped Approach

Think of it this way: instead of searching for the minimum trials for `n` floors, we **grow** the number of floors we can handle one trial at a time.

```
With t trials and k eggs, the "coverage" of floors splits:
  ┌──────────────────────────────────┐
  │  f(t-1, k-1) floors  │  1  │  f(t-1, k) floors  │
  │   (egg breaks)       │floor│  (egg doesn't break)│
  └──────────────────────────────────┘
  Total = f(t-1,k-1) + 1 + f(t-1,k)
```

Each trial perfectly partitions the remaining search space — this is essentially a **decision tree** where each node covers the maximum checkable floors.

---

## Common Mistakes to Avoid

1. **Returning `dp[n-1]` instead of `dp[n]`** — floors are 1-indexed; use `dp[n]` for `n` floors.
    
2. **Not handling `k=1` base case** — with 1 egg, there's no strategy; must check every floor from 1 to n sequentially → answer is `n`.
    
3. **Using linear search instead of binary search** — the inner loop `for x in 1..n` is O(n); replacing it with binary search (exploiting the valley shape) drops it to O(log n).
    
4. **Wrong binary search direction** — when `breakCase < noBreakCase`, the two curves haven't crossed yet (break is still lower), so we need to go **up** (`lo = mid+1`) to balance them.
    
5. **Going left to right in the flipped DP** — `dp[i-1]` must be from the previous `t` iteration. Going right to left preserves this invariant (same trick as 0-1 knapsack).
    
6. **Confusing `f` and `t`** — in the flipped approach, we're fixing trials `t` and finding max floors `f(t,k)`. The answer is the minimum `t` such that `f(t,k) >= n`, not `f(k,n)` directly.
    

---

## Related Problems

| #       | Problem                           | Relationship                      |
| ------- | --------------------------------- | --------------------------------- |
| LC 887  | **Super Egg Drop** ← You are here | Min trials in worst case          |
| LC 312  | Burst Balloons                    | Interval DP — min/max over ranges |
| LC 1039 | Minimum Score Triangulation       | Interval DP                       |
| LC 410  | Split Array Largest Sum           | Binary search on answer           |
| LC 1011 | Capacity to Ship Packages         | Binary search on answer           |