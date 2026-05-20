---
tags:
  - dsa
  - bfs
  - matrix
  - dynamic-programming
  - graphs
  - leetcode
leetcode_id: 542
complexity:
  time: O(m * n)
  space: O(m * n)
status: solved
language: java
---

# 01 Matrix

## Problem Link
- https://leetcode.com/problems/01-matrix/

## Intuition
Distance to nearest 0 can be computed using multi-source BFS.
All 0-cells act as starting points.

## Approach
1. Add all 0-cells to queue.
2. Initialize remaining cells as unvisited.
3. BFS expansion updates shortest distances.

## Java Solution
```java
class Solution {
    public int[][] updateMatrix(int[][] mat) {
        int rows = mat.length;
        int cols = mat[0].length;

        Queue<int[]> queue = new LinkedList<>();

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (mat[i][j] == 0) {
                    queue.offer(new int[]{i, j});
                } else {
                    mat[i][j] = -1;
                }
            }
        }

        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};

        while (!queue.isEmpty()) {
            int[] cell = queue.poll();

            for (int[] d : dirs) {
                int nr = cell[0] + d[0];
                int nc = cell[1] + d[1];

                if (nr >= 0 && nr < rows &&
                    nc >= 0 && nc < cols &&
                    mat[nr][nc] == -1) {

                    mat[nr][nc] = mat[cell[0]][cell[1]] + 1;
                    queue.offer(new int[]{nr, nc});
                }
            }
        }

        return mat;
    }
}
```

## Complexity Analysis
- Time Complexity: O(m * n)
- Space Complexity: O(m * n)

## Key Takeaways
- Multi-source BFS.
- Shortest distance propagation.
