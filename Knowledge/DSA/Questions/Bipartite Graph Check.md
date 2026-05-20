# Bipartite Graph Check

## Tags
- dsa
- graphs
- bfs
- dfs
- leetcode

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;

        int[] color = new int[n];

        for(int i = 0; i < n; i++) {
            if(color[i] != 0) continue;

            Queue<Integer> q = new LinkedList<>();
            q.offer(i);
            color[i] = 1;

            while(!q.isEmpty()) {
                int node = q.poll();

                for(int next : graph[node]) {
                    if(color[next] == 0) {
                        color[next] = -color[node];
                        q.offer(next);
                    } else if(color[next] == color[node]) {
                        return false;
                    }
                }
            }
        }

        return true;
    }
}
```
