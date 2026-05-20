---
tags:
  - dsa
  - matrix
  - simulation
  - arrays
  - leetcode
leetcode_id: 498
complexity:
  time: O(m * n)
  space: O(1)
status: solved
language: java
---

# Diagonal Traverse

## Problem Link
- https://leetcode.com/problems/diagonal-traverse/

## Intuition
Elements on same diagonal share same row + column sum.
Traversal direction alternates every diagonal.

## Approach
1. Simulate movement.
2. Alternate directions:
   - up-right
   - down-left
3. Handle boundary transitions carefully.

## Java Solution
```java
class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int rows = mat.length;
        int cols = mat[0].length;

        int[] answer = new int[rows * cols];

        int r = 0;
        int c = 0;
        int direction = 1;

        for (int i = 0; i < answer.length; i++) {
            answer[i] = mat[r][c];

            if (direction == 1) {
                if (c == cols - 1) {
                    r++;
                    direction = -1;
                } else if (r == 0) {
                    c++;
                    direction = -1;
                } else {
                    r--;
                    c++;
                }
            } else {
                if (r == rows - 1) {
                    c++;
                    direction = 1;
                } else if (c == 0) {
                    r++;
                    direction = 1;
                } else {
                    r++;
                    c--;
                }
            }
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(m * n)
- Space Complexity: O(1)

## Key Takeaways
- Matrix traversal simulation.
- Boundary-driven movement.
