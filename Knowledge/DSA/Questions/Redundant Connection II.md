---
tags:
  - dsa
  - graphs
  - union-find
  - directed-graph
  - leetcode
leetcode_id: 685
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Redundant Connection II

## Problem Link
- https://leetcode.com/problems/redundant-connection-ii/

## Intuition
A node may either:
- have two parents
- form a cycle
- or both

Union-Find helps detect cycles efficiently.

## Approach
1. Detect node with two parents.
2. Temporarily remove one conflicting edge.
3. Run Union-Find.
4. Determine correct removable edge.

## Java Solution
```java
class Solution {
    public int[] findRedundantDirectedConnection(int[][] edges) {
        int n = edges.length;

        int[] parent = new int[n + 1];
        Arrays.fill(parent, 0);

        int[] candidate1 = null;
        int[] candidate2 = null;

        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];

            if (parent[v] == 0) {
                parent[v] = u;
            } else {
                candidate1 = new int[]{parent[v], v};
                candidate2 = new int[]{u, v};
                edge[1] = 0;
            }
        }

        DSU dsu = new DSU(n + 1);

        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];

            if (v == 0) continue;

            if (!dsu.union(u, v)) {
                if (candidate1 == null) {
                    return edge;
                }

                return candidate1;
            }
        }

        return candidate2;
    }

    class DSU {
        int[] parent;

        DSU(int n) {
            parent = new int[n];

            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]);
            }

            return parent[x];
        }

        boolean union(int a, int b) {
            int pa = find(a);
            int pb = find(b);

            if (pa == pb) {
                return false;
            }

            parent[pa] = pb;
            return true;
        }
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Advanced Union-Find problem.
- Handling multiple invalid graph conditions.
