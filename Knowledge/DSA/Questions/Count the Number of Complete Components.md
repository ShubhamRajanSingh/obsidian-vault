---
tags:
  - dsa
  - graph
  - union-find
  - medium
  - leetcode
  - striver
aliases:
  - Count the Number of Complete Components
  - LeetCode 2685
difficulty: Medium
topic: Graph / Union-Find / DFS
pattern: Find connected components, verify completeness
leetcode_id: 2685
date: 2026-05-19
---

# Count the Number of Complete Components

## 📌 Problem Statement

> Given an undirected graph with `n` vertices, return the number of connected components that are complete (every pair of vertices in the component is connected by an edge).

**LeetCode #2685** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (DFS)

```java
class Solution {
    public int countCompleteComponents(int n, int[][] edges) {
        List<Integer>[] adj = new List[n];
        for (int i = 0; i < n; i++) adj[i] = new ArrayList<>();
        for (int[] e : edges) { adj[e[0]].add(e[1]); adj[e[1]].add(e[0]); }

        boolean[] visited = new boolean[n];
        int result = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                List<Integer> comp = new ArrayList<>();
                dfs(adj, visited, i, comp);
                int v = comp.size();
                int e = comp.stream().mapToInt(node -> adj[node].size()).sum() / 2;
                if (e == v * (v - 1) / 2) result++;
            }
        }
        return result;
    }

    private void dfs(List<Integer>[] adj, boolean[] visited, int node, List<Integer> comp) {
        visited[node] = true; comp.add(node);
        for (int nei : adj[node]) if (!visited[nei]) dfs(adj, visited, nei, comp);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(V + E) |
| Space | O(V + E) |
