# Rod Cutting Problem

**Category:** Dynamic Programming **Difficulty:** Medium **Topics:** Array, DP, Unbounded Knapsack **Link:** https://www.geeksforgeeks.org/cutting-a-rod-dp-13/

---

## Problem Statement

Given a rod of length `n` and an array `price[]` where `price[i]` represents the price of a rod of length `i+1`, determine the **maximum revenue** obtainable by cutting the rod into pieces and selling them.

You can cut the rod into any number of pieces (including zero cuts — selling the whole rod). Each length can be used **any number of times** (unbounded).

```
n     = 8
price = [1, 5, 8, 9, 10, 17, 17, 20]
         len=1,2,3,4, 5,  6,  7,  8

Output: 22
Best:   cut into length 2 + length 6 → 5 + 17 = 22
    or: cut into length 6 + length 2 → same
```

---

## Examples

### Example 1

```
n     = 4
price = [2, 5, 7, 8]

Possible cuts:
  4           → price[3] = 8
  1+3         → 2+7 = 9
  2+2         → 5+5 = 10  ✅
  1+1+2       → 2+2+5 = 9
  1+1+1+1     → 2+2+2+2 = 8

Output: 10
```

### Example 2

```
n     = 8
price = [1, 5, 8, 9, 10, 17, 17, 20]

Output: 22  (cut into 2 + 6)
```

---

## Constraints

- `1 <= n <= 1000`
- `1 <= price[i] <= 10^5`

---

## Mapping to Unbounded Knapsack

Rod cutting is a classic **Unbounded Knapsack** problem in disguise:

|Knapsack|Rod Cutting|
|---|---|
|Knapsack capacity `W`|Rod length `n`|
|Item weight `wt[i]`|Piece length `i+1`|
|Item value `val[i]`|Price `price[i]`|
|Each item usable unlimited times|Each length usable unlimited times|
|Maximize total value|Maximize total revenue|

```
lengths = [1, 2, 3, ..., n]   ← weights
prices  = [p1, p2, p3, ..., pn] ← values
capacity = n
```

---

## Intuition

For each cut length `i` (from 1 to n) and remaining rod length `w`, we have two choices:

**Choice 1 — Don't cut length `i`:** Move to the next cut length with the same remaining rod.

```
solve(i-1, w)
```

**Choice 2 — Cut length `i`** (only if `i <= w`): Take price[i-1], reduce remaining rod by `i`, stay at same length (unbounded reuse).

```
price[i-1] + solve(i, w - i)
```

Take the **maximum** of both.

**Base Cases:**

```
solve(i, 0) = 0    (rod length 0 → no revenue)
solve(0, w) = 0    (no lengths to cut → no revenue)
```

---

## Step 1 — Pure Recursion

```java
class RodCutting {
    public int maxRevenue(int[] price, int n) {
        return solve(price, n, n);
    }

    // i = number of lengths to consider (1 to i)
    // w = remaining rod length
    private int solve(int[] price, int i, int w) {
        // Base cases
        if (i == 0 || w == 0) return 0;

        // Length i is more than remaining rod — can't cut this length
        if (i > w) {
            return solve(price, i - 1, w);
        }

        // Max of: don't cut length i  vs  cut length i (stay at i — unbounded)
        int exclude = solve(price, i - 1, w);
        int include = price[i - 1] + solve(price, i, w - i);

        return Math.max(exclude, include);
    }
}
```

### Recursion Tree — `n=4, price=[2,5,7,8]`

```
                  solve(4, 4)
                 /            \
          exclude(4)         include(4) → i=4>w=4? No
          solve(3, 4)        price[3]+solve(4, 0)
          /       \          = 8 + 0 = 8
    excl(3)     incl(3)
  solve(2,4)  price[2]+solve(3,1)
              = 7+solve(3,1)
              ...

solve(2,4):
  excl → solve(1,4)
  incl → price[1]+solve(2,2) = 5+solve(2,2)
    solve(2,2):
      excl → solve(1,2)
      incl → price[1]+solve(2,0) = 5+0 = 5
      solve(1,2):
        incl → price[0]+solve(1,1) = 2+solve(1,1)
          solve(1,1): price[0]+solve(1,0) = 2+0 = 2
        → solve(1,2) = max(0, 2+2) = 4
      → solve(2,2) = max(4, 5) = 5
    → solve(2,4) = max(solve(1,4), 5+5)
      solve(1,4) = price[0]*4 = 2*4 = 8 (all length-1 cuts)
    → solve(2,4) = max(8, 10) = 10  ✅
```

### Problem: Overlapping Subproblems

`solve(i, w)` with the same pair is recomputed multiple times → **O(2^n)** without caching.

---

## Step 2 — Memoization (Top-Down DP)

Cache every `(i, w)` pair. Only `n × (n+1)` unique pairs exist.

```java
class RodCutting {
    int[][] memo;

    public int maxRevenue(int[] price, int n) {
        memo = new int[n + 1][n + 1];
        for (int[] row : memo) Arrays.fill(row, -1);
        return solve(price, n, n);
    }

    private int solve(int[] price, int i, int w) {
        // Base cases
        if (i == 0 || w == 0) return 0;

        // Return cached result
        if (memo[i][w] != -1) return memo[i][w];

        // Length i exceeds remaining rod
        if (i > w) {
            return memo[i][w] = solve(price, i - 1, w);
        }

        int exclude = solve(price, i - 1, w);
        int include = price[i - 1] + solve(price, i, w - i);

        return memo[i][w] = Math.max(exclude, include);
    }
}
```

### What Changes from Recursion?

- Added `memo[n+1][n+1]` initialized to `-1`
- Added cache check: `if (memo[i][w] != -1) return memo[i][w]`
- Added cache store: `return memo[i][w] = ...`
- **Logic and base cases are identical**

### Complexity After Memoization

||Time|Space|
|---|---|---|
|Pure Recursion|O(2^n)|O(n) stack|
|Memoization|O(n²)|O(n²) memo + O(n) stack|

---

## Step 3 — Tabulation (Bottom-Up DP)

`dp[i][w]` = maximum revenue from rod of length `w` using piece lengths `1` to `i`.

### Recurrence

```
If i > w:
    dp[i][w] = dp[i-1][w]                   ← can't cut this length

Else:
    dp[i][w] = max(
        dp[i-1][w],                          ← don't cut length i
        price[i-1] + dp[i][w - i]            ← cut length i (same row — unbounded)
    )
```

### Base Cases

```
dp[i][0] = 0  for all i   (rod length 0 → 0 revenue)
dp[0][w] = 0  for all w   (no lengths → 0 revenue)
```

```java
class RodCutting {
    public int maxRevenue(int[] price, int n) {
        int[][] dp = new int[n + 1][n + 1];

        // Base cases: dp[i][0] = 0, dp[0][w] = 0 (default)

        for (int i = 1; i <= n; i++) {
            for (int w = 1; w <= n; w++) {
                // Can't use piece of length i — too long
                if (i > w) {
                    dp[i][w] = dp[i - 1][w];
                } else {
                    dp[i][w] = Math.max(
                        dp[i - 1][w],                    // exclude length i
                        price[i - 1] + dp[i][w - i]     // include length i (unbounded)
                    );
                }
            }
        }

        return dp[n][n];
    }
}
```

---

## DP Table Visualized

**`n=4, price=[2,5,7,8]`**

```
       w=0  w=1  w=2  w=3  w=4
i=0  [  0    0    0    0    0  ]   no lengths
i=1  [  0    2    4    6    8  ]   length=1, price=2
i=2  [  0    2    5    7   10  ]   length=2, price=5
i=3  [  0    2    5    7   10  ]   length=3, price=7
i=4  [  0    2    5    7   10  ]   length=4, price=8

Answer: dp[4][4] = 10  ✅  (2+2 → price 5+5=10)
```

**Reading key cells:**

- `dp[1][2]`: length=1 ≤ 2 → max(dp[0][2], 2+dp[1][1]) = max(0, 2+2) = 4
- `dp[2][2]`: length=2 ≤ 2 → max(dp[1][2], 5+dp[2][0]) = max(4, 5+0) = 5
- `dp[2][4]`: length=2 ≤ 4 → max(dp[1][4], 5+dp[2][2]) = max(8, 5+5) = 10
- `dp[3][4]`: length=3 ≤ 4 → max(dp[2][4], 7+dp[3][1]) = max(10, 7+2) = 10
- `dp[4][4]`: length=4 ≤ 4 → max(dp[3][4], 8+dp[4][0]) = max(10, 8+0) = 10

---

## Dry Run — Step by Step

**`n=4, price=[2,5,7,8]`**

```
i=1 (length=1, price=2):
  w=1: max(dp[0][1], 2+dp[1][0]) = max(0, 2) = 2
  w=2: max(dp[0][2], 2+dp[1][1]) = max(0, 4) = 4
  w=3: max(dp[0][3], 2+dp[1][2]) = max(0, 6) = 6
  w=4: max(dp[0][4], 2+dp[1][3]) = max(0, 8) = 8
  → [0, 2, 4, 6, 8]

i=2 (length=2, price=5):
  w=1: i>w → dp[1][1] = 2
  w=2: max(dp[1][2], 5+dp[2][0]) = max(4, 5) = 5
  w=3: max(dp[1][3], 5+dp[2][1]) = max(6, 5+2) = 7
  w=4: max(dp[1][4], 5+dp[2][2]) = max(8, 5+5) = 10
  → [0, 2, 5, 7, 10]

i=3 (length=3, price=7):
  w=1: i>w → dp[2][1] = 2
  w=2: i>w → dp[2][2] = 5
  w=3: max(dp[2][3], 7+dp[3][0]) = max(7, 7) = 7
  w=4: max(dp[2][4], 7+dp[3][1]) = max(10, 7+2) = 10
  → [0, 2, 5, 7, 10]

i=4 (length=4, price=8):
  w=1: i>w → dp[3][1] = 2
  w=2: i>w → dp[3][2] = 5
  w=3: i>w → dp[3][3] = 7
  w=4: max(dp[3][4], 8+dp[4][0]) = max(10, 8) = 10
  → [0, 2, 5, 7, 10]

Answer: dp[4][4] = 10  ✅
```

---

## Step 4 — Space Optimized (1D Array)

Since `dp[i][w]` only needs `dp[i-1][w]` and `dp[i][w-i]` (same row), compress to 1D.

**Left to right** traversal — allows reusing the same piece length (unbounded).

```java
class RodCutting {
    public int maxRevenue(int[] price, int n) {
        int[] dp = new int[n + 1];
        // dp[0] = 0 (base case)

        for (int i = 1; i <= n; i++) {
            // Left to right — allows reusing piece of length i
            for (int w = i; w <= n; w++) {
                dp[w] = Math.max(dp[w], price[i - 1] + dp[w - i]);
            }
        }

        return dp[n];
    }
}
```

### 1D Trace — `n=4, price=[2,5,7,8]`

```
Initial: dp = [0, 0, 0, 0, 0]

i=1 (length=1, price=2):
  w=1: max(dp[1], 2+dp[0]) = max(0,2) = 2   → dp=[0,2,0,0,0]
  w=2: max(dp[2], 2+dp[1]) = max(0,4) = 4   → dp=[0,2,4,0,0]
  w=3: max(dp[3], 2+dp[2]) = max(0,6) = 6   → dp=[0,2,4,6,0]
  w=4: max(dp[4], 2+dp[3]) = max(0,8) = 8   → dp=[0,2,4,6,8]

i=2 (length=2, price=5):
  w=2: max(dp[2], 5+dp[0]) = max(4,5) = 5   → dp=[0,2,5,6,8]
  w=3: max(dp[3], 5+dp[1]) = max(6,7) = 7   → dp=[0,2,5,7,8]
  w=4: max(dp[4], 5+dp[2]) = max(8,10)= 10  → dp=[0,2,5,7,10]

i=3 (length=3, price=7):
  w=3: max(dp[3], 7+dp[0]) = max(7,7) = 7   → dp=[0,2,5,7,10]
  w=4: max(dp[4], 7+dp[1]) = max(10,9)= 10  → dp=[0,2,5,7,10]

i=4 (length=4, price=8):
  w=4: max(dp[4], 8+dp[0]) = max(10,8)= 10  → dp=[0,2,5,7,10]

Answer: dp[4] = 10  ✅
```

---

## Reconstructing the Cuts

Trace back through the 2D DP table to find which lengths were cut:

```java
public List<Integer> getCuts(int[] price, int n, int[][] dp) {
    List<Integer> cuts = new ArrayList<>();
    int i = n, w = n;

    while (i > 0 && w > 0) {
        // If value differs from row above, length i was used
        if (dp[i][w] != dp[i - 1][w]) {
            cuts.add(i);         // cut of length i was made
            w -= i;              // reduce remaining rod
            // Don't decrement i — unbounded, can reuse same length
        } else {
            i--;                 // this length was not used
        }
    }

    return cuts;
}
```

**Trace on `n=4, price=[2,5,7,8]`:**

```
DP table:
       w=0  w=1  w=2  w=3  w=4
i=0  [  0    0    0    0    0  ]
i=1  [  0    2    4    6    8  ]
i=2  [  0    2    5    7   10  ]
i=3  [  0    2    5    7   10  ]
i=4  [  0    2    5    7   10  ]

i=4, w=4: dp[4][4]=10 == dp[3][4]=10 → length 4 NOT used → i=3
i=3, w=4: dp[3][4]=10 == dp[2][4]=10 → length 3 NOT used → i=2
i=2, w=4: dp[2][4]=10 ≠ dp[1][4]=8  → length 2 USED → cuts=[2], w=4-2=2
i=2, w=2: dp[2][2]=5  ≠ dp[1][2]=4  → length 2 USED → cuts=[2,2], w=2-2=0

Cuts: [2, 2] → price[1]+price[1] = 5+5 = 10  ✅
```

---

## Progression Summary

|Approach|Time|Space|Notes|
|---|---|---|---|
|Recursion|O(2^n)|O(n)|Exponential — TLE|
|Memoization|O(n²)|O(n²)|Top-down|
|Tabulation|O(n²)|O(n²)|Bottom-up|
|Space Optimized|O(n²)|O(n)|Best; left-to-right traversal|

---

## Rod Cutting vs 0-1 Knapsack vs Coin Change II

||0-1 Knapsack|Rod Cutting|Coin Change II|
|---|---|---|---|
|Reuse|Each item once|Each length unlimited|Each coin unlimited|
|Goal|Maximize value|Maximize revenue|Count combinations|
|Combining|`max(excl, incl)`|`max(excl, incl)`|`excl + incl`|
|Include step|`dp[i-1][w-wt]`|`dp[i][w-i]`|`dp[i][w-coin]`|
|1D traversal|Right to left|**Left to right**|**Left to right**|

---

## Common Mistakes to Avoid

1. **Using `dp[i-1][w-i]` instead of `dp[i][w-i]`** in tabulation — The previous row means each length can only be used once (0-1 knapsack). Use same row `dp[i]` for unbounded rod cutting.
    
2. **Going right to left in 1D** — Right to left prevents reuse, making it a 0-1 knapsack. Always go left to right for unbounded reuse.
    
3. **Wrong price indexing** — Lengths are 1-indexed but `price[]` is 0-indexed. Length `i` has price `price[i-1]`.
    
4. **Forgetting `i > w` guard** — You can't cut a piece of length `i` from a rod of length `w < i`. Without this check you'd access a negative index.
    
5. **Returning `dp[n][n]` from wrong table size** — The DP table is `(n+1) × (n+1)`, not `n × n`. Both dimensions go from `0` to `n`.
    

---

## Related Problems

| #            | Problem          | Relationship                                  |
| ------------ | ---------------- | --------------------------------------------- |
| Rod Cutting  | **You are here** | Unbounded knapsack — maximize revenue         |
| LC 518       | Coin Change II   | Unbounded knapsack — count combinations       |
| LC 322       | Coin Change      | Unbounded knapsack — minimize coin count      |
| LC 343       | Integer Break    | Break n into parts to maximize product        |
| 0-1 Knapsack | Classic DP       | Each item used at most once                   |
| LC 279       | Perfect Squares  | Unbounded — minimize count of perfect squares |