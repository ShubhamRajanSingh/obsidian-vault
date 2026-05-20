---
tags:
  - dsa
  - greedy
  - arrays
  - counting
  - leetcode
leetcode_id: 1007
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Minimum Domino Rotations For Equal Row

## Problem Link
- https://leetcode.com/problems/minimum-domino-rotations-for-equal-row/

## Intuition
The final uniform value must be either:
- tops[0]
- bottoms[0]

Only these candidates can possibly fill every domino.

## Approach
1. Try making all values equal to tops[0].
2. Try making all values equal to bottoms[0].
3. Return minimum valid rotations.

## Java Solution
```java
class Solution {
    public int minDominoRotations(int[] tops, int[] bottoms) {
        int answer = check(tops[0], tops, bottoms);

        if (answer != -1 || tops[0] == bottoms[0]) {
            return answer;
        }

        return check(bottoms[0], tops, bottoms);
    }

    private int check(int target, int[] tops, int[] bottoms) {
        int rotateTop = 0;
        int rotateBottom = 0;

        for (int i = 0; i < tops.length; i++) {
            if (tops[i] != target && bottoms[i] != target) {
                return -1;
            }

            if (tops[i] != target) {
                rotateTop++;
            }

            if (bottoms[i] != target) {
                rotateBottom++;
            }
        }

        return Math.min(rotateTop, rotateBottom);
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Candidate reduction optimization.
- Greedy counting strategy.
