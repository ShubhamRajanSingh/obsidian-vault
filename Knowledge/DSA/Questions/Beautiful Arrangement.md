---
tags:
  - dsa
  - backtracking
  - recursion
  - permutations
  - leetcode
leetcode_id: 526
complexity:
  time: O(n!)
  space: O(n)
status: solved
language: java
---

# Beautiful Arrangement

## Problem Link
- https://leetcode.com/problems/beautiful-arrangement/

## Intuition
At position i, only numbers satisfying divisibility conditions are valid.
Backtracking efficiently explores possibilities.

## Approach
1. Use boolean visited array.
2. Try unused numbers.
3. Validate:
   - num % position == 0
   OR
   - position % num == 0
4. Recursively build arrangements.

## Java Solution
```java
class Solution {

    int count = 0;

    public int countArrangement(int n) {
        boolean[] visited = new boolean[n + 1];

        dfs(1, n, visited);

        return count;
    }

    private void dfs(int position,
                     int n,
                     boolean[] visited) {

        if (position > n) {
            count++;
            return;
        }

        for (int num = 1; num <= n; num++) {
            if (!visited[num] &&
                (num % position == 0 ||
                 position % num == 0)) {

                visited[num] = true;

                dfs(position + 1,
                    n,
                    visited);

                visited[num] = false;
            }
        }
    }
}
```

## Complexity Analysis
- Time Complexity: O(n!)
- Space Complexity: O(n)

## Key Takeaways
- Constraint-based permutation generation.
- Recursive backtracking pruning.
