# Possible Bipartition

## Tags
- dsa
- graphs
- bfs
- dfs
- leetcode

```java
class Solution {
    public boolean possibleBipartition(int n,
                                       int[][] dislikes) {

        List<List<Integer>> graph = new ArrayList<>();

        for(int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }

        for(int[] d : dislikes) {
            graph.get(d[0]).add(d[1]);
            graph.get(d[1]).add(d[0]);
        }

        int[] color = new int[n + 1];

        for(int i = 1; i <= n; i++) {
            if(color[i] != 0) continue;

            Queue<Integer> q = new LinkedList<>();
            q.offer(i);
            color[i] = 1;

            while(!q.isEmpty()) {
                int node = q.poll();

                for(int next : graph.get(node)) {
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
