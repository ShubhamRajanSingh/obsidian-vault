# 0-1 Knapsack Problem

**Category:** Dynamic Programming **Difficulty:** Medium **Topics:** Array, DP, Recursion, Memoization

---

## Problem Statement

You are given `n` items, each with a **weight** `wt[i]` and a **value** `val[i]`, and a knapsack with a maximum weight capacity `W`.

You can either **include** or **exclude** each item (you cannot take fractional amounts or multiple copies of the same item).

Return the **maximum value** you can carry in the knapsack without exceeding the weight capacity.

```
n   = 4
W   = 5
wt  = [1, 2, 3, 5]
val = [1, 6, 10, 16]

Output: 17  (items with weight 2 and 3 → value 6 + 10 = 16... wait)
        Actually: weight 2 (val 6) + weight 3 (val 10) = wt 5, val 16
        Or:       weight 1 (val 1) + weight 2 (val 6) + ... hmm
        Best:     weight 2 (val 6) + weight 3 (val 10) = 16
        Or:       weight 5 (val 16) alone = 16
        Or:       weight 1 (val 1) + weight 2 (val 6) + ... exceeds W=5? 1+2=3 ≤ 5 → val=7, still less
        Answer:   16
```

### Cleaner Example

```
n   = 3
W   = 4
wt  = [1, 3, 4]
val = [1, 4, 5]

Choices:
  Take item 1 (w=1,v=1) + item 2 (w=3,v=4) → total w=4, v=5
  Take item 3 (w=4,v=5) → total w=4, v=5
  Take item 1 (w=1,v=1) alone → v=1

Output: 5
```

---

## Constraints

- `1 <= n <= 1000`
- `1 <= W <= 1000`
- `1 <= wt[i] <= W`
- `1 <= val[i] <= 10^4`

---

## Intuition

For each item `i`, you have exactly **two choices**:

**Choice 1 — Exclude item `i`:** Don't take this item. Move to the next item with the same remaining capacity.

```
solve(i-1, W)
```

**Choice 2 — Include item `i`** (only if `wt[i] <= W`): Take this item. Add its value and reduce remaining capacity by `wt[i]`.

```
val[i] + solve(i-1, W - wt[i])
```

Take the **maximum** of both choices.

**Base cases:**

```
solve(0, W) = 0   (no items left)
solve(i, 0) = 0   (no capacity left)
```

---

## Step 1 — Pure Recursion

```java
class Knapsack {
    public int knapsack(int W, int[] wt, int[] val, int n) {
        return solve(W, wt, val, n);
    }

    private int solve(int W, int[] wt, int[] val, int i) {
        // Base case: no items or no capacity
        if (i == 0 || W == 0) return 0;

        // Item i is too heavy — must exclude
        if (wt[i - 1] > W) {
            return solve(W, wt, val, i - 1);
        }

        // Max of excluding or including item i
        int exclude = solve(W, wt, val, i - 1);
        int include = val[i - 1] + solve(W - wt[i - 1], wt, val, i - 1);

        return Math.max(exclude, include);
    }
}
```

### Recursion Tree — `n=3, W=4, wt=[1,3,4], val=[1,4,5]`

```
                      solve(3, 4)
                     /           \
           exclude(i=3)        include(i=3) wt[2]=4 ≤ 4
           solve(2, 4)          val[2]+solve(2, 0)
          /          \               = 5 + 0 = 5
    exclude(i=2)  include(i=2) wt[1]=3 ≤ 4
    solve(1, 4)   val[1]+solve(1,1)
    /      \       = 4+solve(1,1)
excl    incl      /      \
solve(0,4) val[0]+solve(0,3)  excl    incl
  =0       =1+0=1          solve(0,1) val[0]+solve(0,0)
                              =0        =1+0=1

solve(1,4): max(0,1) = 1
solve(2,4): max(1, 4+1) = 5
solve(3,4): max(5, 5) = 5  ✅
```

### Problem: Overlapping Subproblems

`solve(i, W)` with the same `(i, W)` pair is computed multiple times from different branches → **O(2^n)** worst case.

---

## Step 2 — Memoization (Top-Down DP)

Cache every `(i, W)` pair. Only `n × W` unique pairs exist.

```java
class Knapsack {
    int[][] memo;

    public int knapsack(int W, int[] wt, int[] val, int n) {
        memo = new int[n + 1][W + 1];
        for (int[] row : memo) Arrays.fill(row, -1); // -1 = not computed
        return solve(W, wt, val, n);
    }

    private int solve(int W, int[] wt, int[] val, int i) {
        // Base case
        if (i == 0 || W == 0) return 0;

        // Return cached result
        if (memo[i][W] != -1) return memo[i][W];

        // Item too heavy — must exclude
        if (wt[i - 1] > W) {
            return memo[i][W] = solve(W, wt, val, i - 1);
        }

        int exclude = solve(W, wt, val, i - 1);
        int include = val[i - 1] + solve(W - wt[i - 1], wt, val, i - 1);

        return memo[i][W] = Math.max(exclude, include);
    }
}
```

### What Changes from Recursion?

- Added `memo[n+1][W+1]` initialized to `-1`
- Added cache check: `if (memo[i][W] != -1) return memo[i][W]`
- Added cache store: `return memo[i][W] = ...`
- **Logic and base cases are identical**

### Complexity After Memoization

||Time|Space|
|---|---|---|
|Pure Recursion|O(2^n)|O(n) stack|
|Memoization|O(n × W)|O(n × W) memo + O(n) stack|

---

## Step 3 — Tabulation (Bottom-Up DP)

Build the solution bottom-up. `dp[i][w]` = maximum value using first `i` items with capacity `w`.

### Recurrence

```
If wt[i-1] > w:
    dp[i][w] = dp[i-1][w]             ← can't include, must exclude

Else:
    dp[i][w] = max(
        dp[i-1][w],                   ← exclude item i
        val[i-1] + dp[i-1][w-wt[i-1]] ← include item i
    )
```

### Base Cases

```
dp[0][w] = 0  for all w  (no items → value 0)
dp[i][0] = 0  for all i  (no capacity → value 0)
```

Both are already `0` by default in Java.

```java
class Knapsack {
    public int knapsack(int W, int[] wt, int[] val, int n) {
        int[][] dp = new int[n + 1][W + 1];

        // Base cases: dp[0][w] = 0, dp[i][0] = 0 (default)

        for (int i = 1; i <= n; i++) {
            for (int w = 1; w <= W; w++) {
                // Exclude item i
                dp[i][w] = dp[i - 1][w];

                // Include item i (only if it fits)
                if (wt[i - 1] <= w) {
                    dp[i][w] = Math.max(
                        dp[i][w],
                        val[i - 1] + dp[i - 1][w - wt[i - 1]]
                    );
                }
            }
        }

        return dp[n][W];
    }
}
```

---

## DP Table Visualized

**`n=4, W=5, wt=[2,3,4,5], val=[3,4,5,6]`**

```
       w=0  w=1  w=2  w=3  w=4  w=5
i=0  [  0    0    0    0    0    0  ]   no items
i=1  [  0    0    3    3    3    3  ]   wt=2, val=3
i=2  [  0    0    3    4    4    7  ]   wt=3, val=4
i=3  [  0    0    3    4    5    7  ]   wt=4, val=5
i=4  [  0    0    3    4    5    7  ]   wt=5, val=6

Answer: dp[4][5] = 7  ✅  (items 1+2: wt=2+3=5, val=3+4=7)
```

**Reading key cells:**

- `dp[1][2]`: wt[0]=2 ≤ 2 → max(dp[0][2], 3+dp[0][0]) = max(0, 3) = 3
- `dp[2][3]`: wt[1]=3 ≤ 3 → max(dp[1][3], 4+dp[1][0]) = max(3, 4) = 4
- `dp[2][5]`: wt[1]=3 ≤ 5 → max(dp[1][5], 4+dp[1][2]) = max(3, 4+3) = 7

---

## Dry Run — Step by Step

**`n=3, W=4, wt=[1,3,4], val=[1,4,5]`**

```
       w=0  w=1  w=2  w=3  w=4
i=0  [  0    0    0    0    0  ]

i=1 (wt=1, val=1):
  w=1: wt≤w → max(dp[0][1], 1+dp[0][0]) = max(0, 1) = 1
  w=2: wt≤w → max(dp[0][2], 1+dp[0][1]) = max(0, 1) = 1
  w=3: wt≤w → max(0, 1+0) = 1
  w=4: wt≤w → max(0, 1+0) = 1
i=1  [  0    1    1    1    1  ]

i=2 (wt=3, val=4):
  w=1: wt>w → dp[1][1] = 1
  w=2: wt>w → dp[1][2] = 1
  w=3: wt≤w → max(dp[1][3], 4+dp[1][0]) = max(1, 4) = 4
  w=4: wt≤w → max(dp[1][4], 4+dp[1][1]) = max(1, 4+1) = 5
i=2  [  0    1    1    4    5  ]

i=3 (wt=4, val=5):
  w=1: wt>w → dp[2][1] = 1
  w=2: wt>w → dp[2][2] = 1
  w=3: wt>w → dp[2][3] = 4
  w=4: wt≤w → max(dp[2][4], 5+dp[2][0]) = max(5, 5+0) = 5
i=3  [  0    1    1    4    5  ]

Answer: dp[3][4] = 5  ✅
```

---

## Step 4 — Space Optimized (1D Array)

`dp[i][w]` only depends on `dp[i-1][...]` — the previous row. Compress to a single 1D array.

**Critical:** iterate `w` from **right to left** so we don't use an already-updated value from the current row (which would simulate using the same item twice — that would be unbounded knapsack, not 0-1).

```java
class Knapsack {
    public int knapsack(int W, int[] wt, int[] val, int n) {
        int[] dp = new int[W + 1];

        for (int i = 0; i < n; i++) {
            // Traverse RIGHT TO LEFT to avoid using item i twice
            for (int w = W; w >= wt[i]; w--) {
                dp[w] = Math.max(dp[w], val[i] + dp[w - wt[i]]);
            }
        }

        return dp[W];
    }
}
```

### Why Right to Left?

```
If we go LEFT to RIGHT:
  dp[w] = max(dp[w], val[i] + dp[w - wt[i]])
  When computing dp[5] using dp[3], dp[3] may already be updated
  in the same iteration → item i is counted twice ❌

If we go RIGHT to LEFT:
  dp[w - wt[i]] is always from the PREVIOUS row (not yet updated)
  → item i is counted at most once ✅
```

---

## Reconstructing the Items Chosen

Trace back through the 2D `dp` table:

```java
public List<Integer> getItems(int W, int[] wt, int[] val, int n, int[][] dp) {
    List<Integer> items = new ArrayList<>();
    int w = W;

    for (int i = n; i > 0; i--) {
        // If value changed from row above, item i was included
        if (dp[i][w] != dp[i - 1][w]) {
            items.add(i);           // item i (1-indexed) was taken
            w -= wt[i - 1];        // reduce remaining capacity
        }
    }

    return items;
}
```

**Trace on `n=3, W=4, wt=[1,3,4], val=[1,4,5]`:**

```
dp:
       w=0  w=1  w=2  w=3  w=4
i=0  [  0    0    0    0    0  ]
i=1  [  0    1    1    1    1  ]
i=2  [  0    1    1    4    5  ]
i=3  [  0    1    1    4    5  ]

i=3, w=4: dp[3][4]=5 == dp[2][4]=5 → item 3 NOT taken
i=2, w=4: dp[2][4]=5 ≠ dp[1][4]=1 → item 2 TAKEN, w = 4-3 = 1
i=1, w=1: dp[1][1]=1 ≠ dp[0][1]=0 → item 1 TAKEN, w = 1-1 = 0

Items taken: [1, 2] → wt=1+3=4, val=1+4=5  ✅
```

---

## Progression Summary

|Approach|Time|Space|Notes|
|---|---|---|---|
|Recursion|O(2^n)|O(n)|Exponential — TLE|
|Memoization|O(n × W)|O(n × W)|Top-down|
|Tabulation|O(n × W)|O(n × W)|Bottom-up; standard interview answer|
|Space Optimized|O(n × W)|O(W)|1D array; right-to-left traversal|

---

## 0-1 Knapsack vs Unbounded Knapsack

||0-1 Knapsack|Unbounded Knapsack|
|---|---|---|
|Each item used|At most once|Any number of times|
|1D traversal direction|**Right to left**|**Left to right**|
|Recurrence|`dp[w] = max(dp[w], val[i] + dp[w-wt[i]])`|Same formula|
|Key difference|Previous row values must be preserved|Updated values can be reused|

---

## Common Mistakes to Avoid

1. **Iterating left to right in the 1D approach** — This allows an item to be picked multiple times, turning it into an unbounded knapsack. Always go right to left for 0-1 knapsack.
    
2. **Initializing `memo` with `0` instead of `-1`** — `0` is a valid answer (e.g. no items fit). Use `-1` as the sentinel for "not computed".
    
3. **Wrong index mapping** — `wt[i-1]` and `val[i-1]` when using 1-indexed `dp` (rows go from `1` to `n`, items are 0-indexed in the arrays).
    
4. **Returning `dp[n][W]` vs `max(dp[n])`** — For the standard 0-1 knapsack, the answer is always `dp[n][W]`. Don't take max across the last row (that's needed for different variants).
    
5. **Forgetting the `wt[i-1] > W` guard in recursion** — If the item is heavier than remaining capacity, it must be excluded. Without this check you'd access a negative index in `dp`.
    

---

## Variants Built on 0-1 Knapsack

|Problem|Variation|
|---|---|
|Subset Sum|Can we achieve exactly sum `W`? (`val[i] = wt[i]`, check if `dp[n][W] > 0`)|
|Equal Partition|Can array be split into two equal-sum subsets? (Subset sum with `W = total/2`)|
|Count of Subsets with Given Sum|Count instead of max|
|Minimum Subset Sum Difference|Find partition minimizing `|
|Target Sum (LC 494)|Assign +/- to elements to reach target|

---

## Related Problems

| #       | Problem                    | Relationship                        |
| ------- | -------------------------- | ----------------------------------- |
| LC 416  | Partition Equal Subset Sum | 0-1 Knapsack — can we reach W/2?    |
| LC 494  | Target Sum                 | 0-1 Knapsack variant with +/- signs |
| LC 474  | Ones and Zeroes            | 2D Knapsack (two capacities)        |
| LC 322  | Coin Change                | Unbounded Knapsack (minimize coins) |
| LC 518  | Coin Change II             | Unbounded Knapsack (count ways)     |
| LC 1049 | Last Stone Weight II       | Minimize subset sum difference      |