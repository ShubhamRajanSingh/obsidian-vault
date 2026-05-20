---
tags:
  - dsa
  - backtracking
  - dfs
  - greedy
  - leetcode
leetcode_id: 473
complexity:
  time: O(4^n)
  space: O(n)
status: solved
language: java
---

# Matchsticks to Square

## Problem Link
- https://leetcode.com/problems/matchsticks-to-square/

## Intuition
We need to partition matchsticks into 4 equal sides.
Backtracking explores valid assignments.

## Approach
1. Compute total perimeter.
2. Sort descending for pruning.
3. Try placing stick into each side.
4. Backtrack invalid states.

## Java Solution
```java
class Solution {
    public boolean makesquare(int[] matchsticks) {
        int sum = 0;

        for (int stick : matchsticks) {
            sum += stick;
        }

        if (sum % 4 != 0) {
            return false;
        }

        int side = sum / 4;

        Arrays.sort(matchsticks);

        int[] sides = new int[4];

        return dfs(matchsticks,
                   matchsticks.length - 1,
                   sides,
                   side);
    }

    private boolean dfs(int[] sticks,
                        int index,
                        int[] sides,
                        int target) {

        if (index < 0) {
            return true;
        }

        int value = sticks[index];

        for (int i = 0; i < 4; i++) {
            if (sides[i] + value <= target) {
                sides[i] += value;

                if (dfs(sticks,
                        index - 1,
                        sides,
                        target)) {

                    return true;
                }

                sides[i] -= value;
            }
        }

        return false;
    }
}
```

## Complexity Analysis
- Time Complexity: Exponential
- Space Complexity: O(n)

## Key Takeaways
- Partition backtracking.
- Pruning with descending order.
