---
tags:
  - dsa
  - graphs
  - dfs
  - matrix
  - union-find
  - leetcode
leetcode_id: 827
complexity:
  time: O(n^2)
  space: O(n^2)
status: solved
language: java
---

# Making A Large Island

## Problem Link
- https://leetcode.com/problems/making-a-large-island/

## Intuition
Label every island uniquely and store its size.
Then try converting every 0 into 1.

## Approach
1. DFS label islands.
2. Store island sizes.
3. For each 0:
   - combine neighboring unique islands.
4. Track maximum size.

## Java Solution
```java
class Solution {
    int n;
    int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};

    public int largestIsland(int[][] grid) {
        n = grid.length;

        Map<Integer, Integer> sizeMap = new HashMap<>();
        int id = 2;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    sizeMap.put(id, dfs(grid, i, j, id));
                    id++;
                }
            }
        }

        int answer = 0;

        for (int size : sizeMap.values()) {
            answer = Math.max(answer, size);
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) {
                    Set<Integer> seen = new HashSet<>();
                    int size = 1;

                    for (int[] d : dirs) {
                        int nr = i + d[0];
                        int nc = j + d[1];

                        if (nr >= 0 && nr < n &&
                            nc >= 0 && nc < n &&
                            grid[nr][nc] > 1) {

                            int islandId = grid[nr][nc];

                            if (seen.add(islandId)) {
                                size += sizeMap.get(islandId);
                            }
                        }
                    }

                    answer = Math.max(answer, size);
                }
            }
        }

        return answer == 0 ? n * n : answer;
    }

    private int dfs(int[][] grid, int r, int c, int id) {
        if (r < 0 || r >= n || c < 0 || c >= n || grid[r][c] != 1) {
            return 0;
        }

        grid[r][c] = id;

        int size = 1;

        for (int[] d : dirs) {
            size += dfs(grid, r + d[0], c + d[1], id);
        }

        return size;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^2)
- Space Complexity: O(n^2)

## Key Takeaways
- Island labeling technique.
- Component merging optimization.
