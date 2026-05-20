# Find Eventual Safe States

## Tags
- dsa
- graphs
- dfs
- topological-sort
- leetcode

```java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;

        int[] state = new int[n];
        List<Integer> ans = new ArrayList<>();

        for(int i = 0; i < n; i++) {
            if(dfs(graph, i, state)) {
                ans.add(i);
            }
        }

        return ans;
    }

    private boolean dfs(int[][] graph,
                        int node,
                        int[] state) {

        if(state[node] != 0) {
            return state[node] == 2;
        }

        state[node] = 1;

        for(int next : graph[node]) {
            if(!dfs(graph, next, state)) {
                return false;
            }
        }

        state[node] = 2;
        return true;
    }
}
```
