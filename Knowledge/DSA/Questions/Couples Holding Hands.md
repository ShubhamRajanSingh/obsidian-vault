---
tags:
  - dsa
  - greedy
  - graph
  - union-find
  - leetcode
leetcode_id: 765
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Couples Holding Hands

## Problem Link
- https://leetcode.com/problems/couples-holding-hands/

## Intuition
Each couple should sit together.
Greedily swap incorrect partners into correct positions.

## Approach
1. Store index of every person.
2. Traverse pairs.
3. Compute expected partner.
4. Swap if necessary.

## Java Solution
```java
class Solution {
    public int minSwapsCouples(int[] row) {
        int n = row.length;

        int[] position = new int[n];

        for (int i = 0; i < n; i++) {
            position[row[i]] = i;
        }

        int swaps = 0;

        for (int i = 0; i < n; i += 2) {
            int first = row[i];
            int partner = first ^ 1;

            if (row[i + 1] == partner) {
                continue;
            }

            swaps++;

            int partnerIndex = position[partner];

            position[row[i + 1]] = partnerIndex;

            row[partnerIndex] = row[i + 1];
            row[i + 1] = partner;
        }

        return swaps;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Greedy swap correction.
- XOR trick for partner lookup.
