---
tags:
  - dsa
  - graph
  - bfs
  - dfs
  - union-find
  - medium
  - leetcode
  - striver
aliases:
  - Number of Islands
  - LeetCode 200
difficulty: Medium
topic: Graph / BFS / DFS / Union-Find
pattern: DFS flood fill
leetcode_id: 200
date: 2026-05-19
---

# Number of Islands

## 📌 Problem Statement

> Given an m×n grid of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and formed by connecting adjacent lands horizontally or vertically.

**LeetCode #200** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (DFS)

```java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for (int i = 0; i < grid.length; i++)
            for (int j = 0; j < grid[0].length; j++)
                if (grid[i][j] == '1') { dfs(grid, i, j); count++; }
        return count;
    }

    private void dfs(char[][] grid, int i, int j) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != '1') return;
        grid[i][j] = '0';
        dfs(grid, i+1, j); dfs(grid, i-1, j);
        dfs(grid, i, j+1); dfs(grid, i, j-1);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(m × n) |
| Space | O(m × n) worst case stack |
