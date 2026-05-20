# All Paths From Source to Target

## Tags
- dsa
- dfs
- graphs
- backtracking
- leetcode

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> path = new ArrayList<>();

        path.add(0);
        dfs(graph, 0, path, ans);

        return ans;
    }

    private void dfs(int[][] graph,
                     int node,
                     List<Integer> path,
                     List<List<Integer>> ans) {

        if (node == graph.length - 1) {
            ans.add(new ArrayList<>(path));
            return;
        }

        for (int next : graph[node]) {
            path.add(next);
            dfs(graph, next, path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```
