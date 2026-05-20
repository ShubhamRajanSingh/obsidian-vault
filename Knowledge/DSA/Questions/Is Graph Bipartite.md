---
tags:
  - dsa
  - graph
  - bfs
  - medium
  - leetcode
  - striver
aliases:
  - Is Graph Bipartite?
  - LeetCode 785
difficulty: Medium
topic: Graph / BFS / Coloring
pattern: BFS 2-coloring
leetcode_id: 785
date: 2026-05-19
---

# Is Graph Bipartite?

## 📌 Problem Statement

> Given an undirected graph, return `true` if the graph is bipartite (nodes can be split into two independent sets such that every edge connects one set to the other).

**LeetCode #785** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (BFS Coloring)

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n]; // 0 = unvisited, 1 or -1 = colors

        for (int i = 0; i < n; i++) {
            if (color[i] != 0) continue;
            Queue<Integer> q = new LinkedList<>();
            q.offer(i); color[i] = 1;
            while (!q.isEmpty()) {
                int node = q.poll();
                for (int nei : graph[node]) {
                    if (color[nei] == 0) { color[nei] = -color[node]; q.offer(nei); }
                    else if (color[nei] == color[node]) return false;
                }
            }
        }
        return true;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(V + E) |
| Space | O(V) |
