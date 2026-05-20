---
tags:
  - dsa
  - game-theory
  - bitmask
  - dynamic-programming
  - recursion
  - leetcode
leetcode_id: 464
complexity:
  time: O(2^n * n)
  space: O(2^n)
status: solved
language: java
---

# Can I Win

## Problem Link
- https://leetcode.com/problems/can-i-win/

## Intuition
Each game state depends on:
- chosen numbers
- remaining target

Memoized recursion avoids recomputation.

## Approach
1. Represent used numbers with bitmask.
2. Try every unused number.
3. If opponent loses after current move:
   current player wins.
4. Memoize computed states.

## Java Solution
```java
class Solution {

    Map<Integer, Boolean> memo = new HashMap<>();

    public boolean canIWin(int maxChoosableInteger,
                           int desiredTotal) {

        int totalSum =
            (maxChoosableInteger *
            (maxChoosableInteger + 1)) / 2;

        if (totalSum < desiredTotal) {
            return false;
        }

        return dfs(maxChoosableInteger,
                   desiredTotal,
                   0);
    }

    private boolean dfs(int max,
                        int target,
                        int mask) {

        if (target <= 0) {
            return false;
        }

        if (memo.containsKey(mask)) {
            return memo.get(mask);
        }

        for (int i = 1; i <= max; i++) {
            int bit = 1 << i;

            if ((mask & bit) == 0) {
                if (!dfs(max,
                         target - i,
                         mask | bit)) {

                    memo.put(mask, true);
                    return true;
                }
            }
        }

        memo.put(mask, false);
        return false;
    }
}
```

## Complexity Analysis
- Time Complexity: O(2^n * n)
- Space Complexity: O(2^n)

## Key Takeaways
- Minimax recursion.
- Bitmask state compression.
- Memoization optimization.
