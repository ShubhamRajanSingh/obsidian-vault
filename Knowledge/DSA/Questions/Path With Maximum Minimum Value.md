---
tags:
  - dsa
  - graphs
  - dijkstra
  - matrix
  - heap
  - leetcode
leetcode_id: 1102
complexity:
  time: O(m*n log(m*n))
  space: O(m*n)
status: solved
language: java
---

# Path With Maximum Minimum Value

## Problem Link
- https://leetcode.com/problems/path-with-maximum-minimum-value/

## Intuition
We want a path maximizing the minimum value encountered.
Priority queue helps greedily expand strongest path first.

## Approach
1. Use max-heap.
2. Track minimum value along current path.
3. Expand highest candidate path.
4. Return value at destination.

## Java Solution
```java
class Solution {
    public int maximumMinimumPath(int[][] grid) {
        int rows = grid.length;
        int cols = grid[0].length;

        PriorityQueue<int[]> pq = new PriorityQueue<>(
            (a, b) -> b[0] - a[0]
        );

        boolean[][] visited = new boolean[rows][cols];

        pq.offer(new int[]{grid[0][0], 0, 0});

        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};

        while (!pq.isEmpty()) {
            int[] current = pq.poll();

            int score = current[0];
            int r = current[1];
            int c = current[2];

            if (visited[r][c]) {
                continue;
            }

            visited[r][c] = true;

            if (r == rows - 1 && c == cols - 1) {
                return score;
            }

            for (int[] d : dirs) {
                int nr = r + d[0];
                int nc = c + d[1];

                if (nr >= 0 && nr < rows &&
                    nc >= 0 && nc < cols &&
                    !visited[nr][nc]) {

                    pq.offer(new int[]{
                        Math.min(score, grid[nr][nc]),
                        nr,
                        nc
                    });
                }
            }
        }

        return -1;
    }
}
```

## Complexity Analysis
- Time Complexity: O(m*n log(m*n))
- Space Complexity: O(m*n)

## Key Takeaways
- Maximum bottleneck path.
- Priority queue graph traversal.
