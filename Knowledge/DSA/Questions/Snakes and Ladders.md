---
tags:
  - dsa
  - bfs
  - graphs
  - matrix
  - leetcode
leetcode_id: 909
complexity:
  time: O(n^2)
  space: O(n^2)
status: solved
language: java
---

# Snakes and Ladders

## Problem Link
- https://leetcode.com/problems/snakes-and-ladders/

## Intuition
Each dice throw creates graph edges.
Minimum moves problem naturally maps to BFS.

## Approach
1. Convert board positions.
2. BFS traversal.
3. Explore next 6 moves.
4. Handle snake/ladder jumps.

## Java Solution
```java
class Solution {
    public int snakesAndLadders(int[][] board) {
        int n = board.length;

        Queue<Integer> queue = new LinkedList<>();
        boolean[] visited = new boolean[n * n + 1];

        queue.offer(1);
        visited[1] = true;

        int moves = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();

            for (int i = 0; i < size; i++) {
                int current = queue.poll();

                if (current == n * n) {
                    return moves;
                }

                for (int next = current + 1;
                     next <= Math.min(current + 6, n * n);
                     next++) {

                    int[] pos = getPosition(next, n);
                    int r = pos[0];
                    int c = pos[1];

                    int destination = board[r][c] == -1
                            ? next
                            : board[r][c];

                    if (!visited[destination]) {
                        visited[destination] = true;
                        queue.offer(destination);
                    }
                }
            }

            moves++;
        }

        return -1;
    }

    private int[] getPosition(int num, int n) {
        int row = (num - 1) / n;
        int col = (num - 1) % n;

        if (row % 2 == 1) {
            col = n - 1 - col;
        }

        return new int[]{n - 1 - row, col};
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^2)
- Space Complexity: O(n^2)

## Key Takeaways
- BFS shortest path.
- Coordinate transformation trick.
