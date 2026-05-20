---
tags:
  - dsa
  - game-theory
  - graphs
  - bfs
  - dynamic-programming
  - leetcode
leetcode_id: 913
complexity:
  time: O(n^3)
  space: O(n^3)
status: solved
language: java
---

# Cat and Mouse

## Problem Link
- https://leetcode.com/problems/cat-and-mouse/

## Intuition
This is a game-state graph problem.
Each state depends on future winning/losing states.
Reverse BFS/topological reasoning solves it.

## Approach
1. Define state:
   - mouse position
   - cat position
   - turn
2. Initialize terminal states.
3. Reverse propagate winning/losing states.
4. Determine result of initial state.

## Java Solution
```java
class Solution {
    static final int DRAW = 0;
    static final int MOUSE = 1;
    static final int CAT = 2;

    public int catMouseGame(int[][] graph) {
        int n = graph.length;

        int[][][] color = new int[n][n][3];
        int[][][] degree = new int[n][n][3];

        for (int m = 0; m < n; m++) {
            for (int c = 0; c < n; c++) {
                degree[m][c][1] = graph[m].length;
                degree[m][c][2] = graph[c].length;

                for (int next : graph[c]) {
                    if (next == 0) {
                        degree[m][c][2]--;
                    }
                }
            }
        }

        Queue<int[]> queue = new LinkedList<>();

        for (int i = 1; i < n; i++) {
            color[0][i][1] = MOUSE;
            color[0][i][2] = MOUSE;

            queue.offer(new int[]{0, i, 1, MOUSE});
            queue.offer(new int[]{0, i, 2, MOUSE});

            color[i][i][1] = CAT;
            color[i][i][2] = CAT;

            queue.offer(new int[]{i, i, 1, CAT});
            queue.offer(new int[]{i, i, 2, CAT});
        }

        while (!queue.isEmpty()) {
            int[] state = queue.poll();

            int mouse = state[0];
            int cat = state[1];
            int turn = state[2];
            int result = state[3];

            for (int[] prev : parents(graph, mouse, cat, turn)) {
                int pm = prev[0];
                int pc = prev[1];
                int pt = prev[2];

                if (color[pm][pc][pt] != DRAW) {
                    continue;
                }

                if ((pt == 1 && result == MOUSE) ||
                    (pt == 2 && result == CAT)) {

                    color[pm][pc][pt] = result;
                    queue.offer(new int[]{pm, pc, pt, result});

                } else {
                    degree[pm][pc][pt]--;

                    if (degree[pm][pc][pt] == 0) {
                        int lose = pt == 1 ? CAT : MOUSE;

                        color[pm][pc][pt] = lose;
                        queue.offer(new int[]{pm, pc, pt, lose});
                    }
                }
            }
        }

        return color[1][2][1];
    }

    private List<int[]> parents(int[][] graph,
                                int mouse,
                                int cat,
                                int turn) {

        List<int[]> result = new ArrayList<>();

        if (turn == 2) {
            for (int prev : graph[mouse]) {
                result.add(new int[]{prev, cat, 1});
            }
        } else {
            for (int prev : graph[cat]) {
                if (prev != 0) {
                    result.add(new int[]{mouse, prev, 2});
                }
            }
        }

        return result;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^3)
- Space Complexity: O(n^3)

## Key Takeaways
- Game state graph DP.
- Reverse BFS propagation.
