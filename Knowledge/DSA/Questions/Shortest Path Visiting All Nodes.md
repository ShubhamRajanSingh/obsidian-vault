# Shortest Path Visiting All Nodes

## Tags
- dsa
- bfs
- bitmask
- graphs
- leetcode

```java
class Solution {
    public int shortestPathLength(int[][] graph) {
        int n = graph.length;

        Queue<int[]> q = new LinkedList<>();
        boolean[][] visited = new boolean[n][1 << n];

        for(int i = 0; i < n; i++) {
            int mask = 1 << i;
            q.offer(new int[]{i, mask});
            visited[i][mask] = true;
        }

        int steps = 0;
        int target = (1 << n) - 1;

        while(!q.isEmpty()) {
            int size = q.size();

            for(int i = 0; i < size; i++) {
                int[] cur = q.poll();

                int node = cur[0], mask = cur[1];

                if(mask == target) return steps;

                for(int next : graph[node]) {
                    int nextMask = mask | (1 << next);

                    if(!visited[next][nextMask]) {
                        visited[next][nextMask] = true;
                        q.offer(new int[]{next, nextMask});
                    }
                }
            }

            steps++;
        }

        return -1;
    }
}
```
