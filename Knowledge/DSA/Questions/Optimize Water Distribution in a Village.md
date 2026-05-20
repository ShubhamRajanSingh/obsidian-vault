---
tags:
  - dsa
  - graphs
  - minimum-spanning-tree
  - union-find
  - greedy
  - leetcode
leetcode_id: 1168
complexity:
  time: O(E log E)
  space: O(V)
status: solved
language: java
---

# Optimize Water Distribution in a Village

## Problem Link
- https://leetcode.com/problems/optimize-water-distribution-in-a-village/

## Intuition
Treat wells as edges from a virtual node.
Then solve the entire problem using Minimum Spanning Tree.

## Approach
1. Add virtual node 0.
2. Connect node 0 to each house with well cost.
3. Add all pipe connections.
4. Sort edges by cost.
5. Apply Kruskal's Algorithm.

## Java Solution
```java
class Solution {
    public int minCostToSupplyWater(int n,
                                    int[] wells,
                                    int[][] pipes) {

        List<int[]> edges = new ArrayList<>();

        for (int i = 0; i < wells.length; i++) {
            edges.add(new int[]{0, i + 1, wells[i]});
        }

        for (int[] pipe : pipes) {
            edges.add(pipe);
        }

        edges.sort((a, b) -> a[2] - b[2]);

        DSU dsu = new DSU(n + 1);

        int answer = 0;

        for (int[] edge : edges) {
            if (dsu.union(edge[0], edge[1])) {
                answer += edge[2];
            }
        }

        return answer;
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
- Time Complexity: O(E log E)
- Space Complexity: O(V)

## Key Takeaways
- Virtual node transformation.
- Kruskal MST modeling.
