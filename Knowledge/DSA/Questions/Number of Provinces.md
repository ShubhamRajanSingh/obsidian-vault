---
tags:
  - dsa
  - graph
  - union-find
  - medium
  - leetcode
  - striver
aliases:
  - Number of Provinces
  - LeetCode 547
difficulty: Medium
topic: Graph / Union-Find / DFS
pattern: DFS/BFS connected components
leetcode_id: 547
date: 2026-05-19
---

# Number of Provinces

## 📌 Problem Statement

> Given an n×n adjacency matrix `isConnected`, find the total number of provinces (connected components).

**LeetCode #547** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (DFS)

```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int provinces = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) { dfs(isConnected, visited, i); provinces++; }
        }
        return provinces;
    }

    private void dfs(int[][] g, boolean[] visited, int i) {
        visited[i] = true;
        for (int j = 0; j < g.length; j++)
            if (g[i][j] == 1 && !visited[j]) dfs(g, visited, j);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n²) |
| Space | O(n) |
