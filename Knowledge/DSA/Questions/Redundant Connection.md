---
tags:
  - dsa
  - union-find
  - graphs
  - trees
  - leetcode
leetcode_id: 684
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Redundant Connection

## Problem Link
- https://leetcode.com/problems/redundant-connection/

## Intuition
A redundant edge creates a cycle.
Union-Find efficiently detects cycles.

## Approach
1. Process edges sequentially.
2. If two nodes already belong to same component:
   edge is redundant.
3. Otherwise union them.

## Java Solution
```java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;

        int[] parent = new int[n + 1];

        for (int i = 1; i <= n; i++) {
            parent[i] = i;
        }

        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];

            int pu = find(parent, u);
            int pv = find(parent, v);

            if (pu == pv) {
                return edge;
            }

            parent[pu] = pv;
        }

        return new int[0];
    }

    private int find(int[] parent, int x) {
        if (parent[x] != x) {
            parent[x] = find(parent, parent[x]);
        }

        return parent[x];
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Cycle detection using DSU.
- Incremental graph building.
