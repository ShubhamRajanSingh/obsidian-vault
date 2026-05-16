# 300. Longest Increasing Subsequence

**Difficulty:** Medium **Topics:** Array, Dynamic Programming, Binary Search **Link:** https://leetcode.com/problems/longest-increasing-subsequence/

---

## Problem Statement

Given an integer array `nums`, return the length of the **longest strictly increasing subsequence**.

A **subsequence** is a sequence derived from the array by deleting some or no elements without changing the relative order of the remaining elements.

```
nums = [10, 9, 2, 5, 3, 7, 101, 18]
Output: 4
LIS = [2, 3, 7, 101] or [2, 5, 7, 101]
```

---

## Examples

### Example 1

```
Input:  nums = [10, 9, 2, 5, 3, 7, 101, 18]
Output: 4
```

### Example 2

```
Input:  nums = [0, 1, 0, 3, 2, 3]
Output: 4
```

### Example 3

```
Input:  nums = [7, 7, 7, 7, 7]
Output: 1
```

---

## Constraints

- `1 <= nums.length <= 2500`
- `-10^4 <= nums[i] <= 10^4`

---

## Intuition

For every element `nums[i]`, ask:

> What is the longest increasing subsequence **ending at index `i`**?

To answer this, look at all previous elements `nums[j]` where `j < i`. If `nums[j] < nums[i]`, then `nums[i]` can extend the subsequence ending at `j`.

```
LIS ending at i = 1 + max(LIS ending at j)
                  for all j < i where nums[j] < nums[i]
```

The overall answer is the maximum across all positions.

---

## Step 1 — Pure Recursion

Define `solve(i)` = length of LIS ending at index `i`.

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int max = 1;
        for (int i = 0; i < nums.length; i++) {
            max = Math.max(max, solve(nums, i));
        }
        return max;
    }

    private int solve(int[] nums, int i) {
        // Base: LIS of length at least 1 (just the element itself)
        int best = 1;

        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                // nums[i] can extend the subsequence ending at j
                best = Math.max(best, 1 + solve(nums, j));
            }
        }

        return best;
    }
}
```

### Recursion Tree (partial) — `[2, 5, 3, 7]`

```
solve(3) → nums[3]=7
  j=0: nums[0]=2 < 7 → 1 + solve(0)
  j=1: nums[1]=5 < 7 → 1 + solve(1)
  j=2: nums[2]=3 < 7 → 1 + solve(2)

solve(1) → nums[1]=5
  j=0: nums[0]=2 < 5 → 1 + solve(0)

solve(2) → nums[2]=3
  j=0: nums[0]=2 < 3 → 1 + solve(0)  ← solve(0) computed again!
```

### Problem: Overlapping Subproblems

`solve(0)` is computed multiple times from different branches → **O(2^n)** worst case.

---

## Step 2 — Memoization (Top-Down DP)

Cache `solve(i)` — only `n` unique values exist.

```java
class Solution {
    int[] memo;

    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        memo = new int[n];
        Arrays.fill(memo, -1); // -1 = not yet computed

        int max = 1;
        for (int i = 0; i < n; i++) {
            max = Math.max(max, solve(nums, i));
        }
        return max;
    }

    private int solve(int[] nums, int i) {
        // Return cached result
        if (memo[i] != -1) return memo[i];

        int best = 1;
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                best = Math.max(best, 1 + solve(nums, j));
            }
        }

        return memo[i] = best;
    }
}
```

### What Changes from Recursion?

- Added `memo[]` initialized to `-1`
- Added cache check: `if (memo[i] != -1) return memo[i]`
- Added cache store: `return memo[i] = best`
- **Logic is identical**

### Complexity After Memoization

||Time|Space|
|---|---|---|
|Pure Recursion|O(2^n)|O(n) stack|
|Memoization|O(n²)|O(n) memo + O(n) stack|

---

## Step 3 — Tabulation (Bottom-Up DP)

Convert top-down to bottom-up. `dp[i]` = length of LIS ending at index `i`.

```
Base case:  dp[i] = 1  for all i  (every element alone is an LIS of length 1)

Recurrence: for each i from 0 to n-1:
              for each j from 0 to i-1:
                if nums[j] < nums[i]:
                  dp[i] = max(dp[i], dp[j] + 1)

Answer: max(dp[0], dp[1], ..., dp[n-1])
```

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1); // every element is an LIS of length 1

        int max = 1;

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            max = Math.max(max, dp[i]);
        }

        return max;
    }
}
```

---

## DP Table Trace

**nums = `[10, 9, 2, 5, 3, 7, 101, 18]`**

|i|nums[i]|Valid j's (nums[j] < nums[i])|dp[i]|
|---|---|---|---|
|0|10|none|1|
|1|9|none|1|
|2|2|none|1|
|3|5|j=2 (2<5): dp[2]+1=2|2|
|4|3|j=2 (2<3): dp[2]+1=2|2|
|5|7|j=2(2<7):2, j=3(5<7):3, j=4(3<7):3|**3**|
|6|101|j=0..5 all < 101: max=dp[5]+1=4|**4**|
|7|18|j=2,3,4,5 valid: max=dp[5]+1=4|**4**|

```
dp  = [1, 1, 1, 2, 2, 3, 4, 4]
max = 4  ✅
```

LIS: `[2, 3, 7, 101]` or `[2, 5, 7, 101]`

---

## Progression Summary — O(n²) approaches

|Approach|Time|Space|Notes|
|---|---|---|---|
|Recursion|O(2^n)|O(n)|Exponential — TLE|
|Memoization|O(n²)|O(n)|Top-down|
|Tabulation|O(n²)|O(n)|Bottom-up; cleaner|

---

## Step 4 — Binary Search (Patience Sorting) — O(n log n)

Can we do better than O(n²)? Yes — using a **greedy approach with binary search**.

### Idea

Maintain a `tails` array where `tails[i]` is the **smallest possible tail element** of all increasing subsequences of length `i+1`.

For each `num` in `nums`:

- If `num` is **greater than all tails** → extend the longest subsequence → append to `tails`
- Otherwise → find the **leftmost tail ≥ num** → replace it with `num` (a smaller tail means future elements are more likely to extend this subsequence)

The answer is the length of `tails`.

### Why Replace?

Replacing a tail with a smaller value doesn't change the current LIS length — but it makes the subsequence **easier to extend** in the future. A smaller tail leaves more room for future elements.

```
nums = [10, 9, 2, 5, 3, 7, 101, 18]

num=10: tails=[]     → 10 > nothing → append   → tails=[10]
num=9:  tails=[10]   → 9 < 10 → replace 10     → tails=[9]
num=2:  tails=[9]    → 2 < 9  → replace 9      → tails=[2]
num=5:  tails=[2]    → 5 > 2  → append         → tails=[2,5]
num=3:  tails=[2,5]  → 3 < 5  → replace 5      → tails=[2,3]
num=7:  tails=[2,3]  → 7 > 3  → append         → tails=[2,3,7]
num=101:tails=[2,3,7]→ 101>7  → append         → tails=[2,3,7,101]
num=18: tails=[2,3,7,101]→18<101→replace 101   → tails=[2,3,7,18]

Length of tails = 4  ✅
```

> **Note:** `tails` is NOT the actual LIS — it's a bookkeeping structure. The length of `tails` gives the correct LIS length, but the elements may not form a valid subsequence.

### Binary Search: Find Leftmost Position ≥ num

Since `tails` is always sorted in increasing order, use binary search to find the insertion point:

```java
// Standard lower_bound: first index where tails[mid] >= num
int lo = 0, hi = tails.size();
while (lo < hi) {
    int mid = lo + (hi - lo) / 2;
    if (tails.get(mid) < num) lo = mid + 1;
    else hi = mid;
}
// lo is the position to replace (or append if lo == tails.size())
```

### Java Solution — O(n log n)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        List<Integer> tails = new ArrayList<>();

        for (int num : nums) {
            int pos = lowerBound(tails, num);

            if (pos == tails.size()) {
                tails.add(num);      // num extends the longest subsequence
            } else {
                tails.set(pos, num); // replace to keep tail as small as possible
            }
        }

        return tails.size();
    }

    // First index in tails where tails[index] >= num
    private int lowerBound(List<Integer> tails, int num) {
        int lo = 0, hi = tails.size();
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            if (tails.get(mid) < num) lo = mid + 1;
            else hi = mid;
        }
        return lo;
    }
}
```

### Using Java's Built-in Binary Search

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];
        int size = 0;

        for (int num : nums) {
            // Arrays.binarySearch returns -(insertion point) - 1 if not found
            int pos = Arrays.binarySearch(tails, 0, size, num);
            if (pos < 0) pos = -(pos + 1); // convert to insertion point

            tails[pos] = num;
            if (pos == size) size++;
        }

        return size;
    }
}
```

---

## Full Comparison

|Approach|Time|Space|Notes|
|---|---|---|---|
|Recursion|O(2^n)|O(n)|Exponential — TLE|
|Memoization|O(n²)|O(n)|Top-down DP|
|Tabulation|O(n²)|O(n)|Bottom-up DP; interview standard|
|Binary Search|**O(n log n)**|O(n)|Optimal; harder to intuit|

---

## Reconstructing the Actual LIS

The tabulation approach makes reconstruction straightforward — trace back through `dp[]` and `nums[]`:

```java
public List<Integer> reconstructLIS(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    int[] parent = new int[n]; // parent[i] = index before i in LIS
    Arrays.fill(dp, 1);
    Arrays.fill(parent, -1);

    int maxLen = 1, maxIdx = 0;

    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i] && dp[j] + 1 > dp[i]) {
                dp[i] = dp[j] + 1;
                parent[i] = j;
            }
        }
        if (dp[i] > maxLen) {
            maxLen = dp[i];
            maxIdx = i;
        }
    }

    // Trace back using parent array
    LinkedList<Integer> lis = new LinkedList<>();
    for (int i = maxIdx; i != -1; i = parent[i]) {
        lis.addFirst(nums[i]);
    }

    return lis;
}
```

**Trace on `[10, 9, 2, 5, 3, 7, 101, 18]`:**

```
dp     = [1, 1, 1, 2, 2, 3, 4, 4]
parent = [-1,-1,-1, 2, 2, 4, 5, 5]

maxIdx = 6 (dp[6]=4)
Trace: 6 → 5 → 4 → 2
Values: nums[6]=101, nums[5]=7, nums[4]=3, nums[2]=2
LIS = [2, 3, 7, 101]  ✅
```

---

## Common Mistakes to Avoid

1. **Initializing `dp[i] = 0` instead of `1`** — Every element is by itself a valid LIS of length 1. Starting at 0 underestimates the result.
    
2. **Using `>=` instead of `<` in the condition** — The problem asks for **strictly increasing**. Use `nums[j] < nums[i]`, not `<=`.
    
3. **Returning `dp[n-1]` instead of `max(dp)`** — The LIS doesn't have to end at the last element. Always track and return the global maximum.
    
4. **Thinking `tails` is the actual LIS** — In the binary search approach, `tails` is a bookkeeping array. Its length is correct but its contents may not form a valid subsequence.
    
5. **Using `lowerBound` vs `upperBound`** — For strictly increasing, use `lowerBound` (first index `>= num`) so duplicates replace the existing equal element, not extend the subsequence.
    

---

## Related Problems

| #       | Problem                                           | Relationship                            |
| ------- | ------------------------------------------------- | --------------------------------------- |
| LC 300  | **Longest Increasing Subsequence** ← You are here | Classic LIS                             |
| LC 673  | Number of LIS                                     | Count all LIS of maximum length         |
| LC 354  | Russian Doll Envelopes                            | 2D LIS (sort + LIS on one dimension)    |
| LC 1143 | Longest Common Subsequence                        | Related DP on two sequences             |
| LC 368  | Largest Divisible Subset                          | LIS variant with divisibility condition |
| LC 646  | Maximum Length of Pair Chain                      | LIS on intervals                        |