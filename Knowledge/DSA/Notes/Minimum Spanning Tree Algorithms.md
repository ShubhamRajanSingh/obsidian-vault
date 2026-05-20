# Minimum Spanning Tree Algorithms

## Tags
- dsa
- graphs
- mst
- greedy
- union-find

# Minimum Spanning Tree (MST)
A Minimum Spanning Tree is a subset of edges that:
- Connects all vertices
- Has no cycles
- Has minimum possible total edge weight

---

# Prim's Algorithm

## Idea
Prim’s Algorithm grows the MST one node at a time.

It always selects the minimum weighted edge connecting visited and unvisited nodes.

## Time Complexity
- O(E log V)

## Java Code

```java
import java.util.*;

class PrimsAlgorithm {

    static class Pair {
        int node;
        int weight;

        Pair(int node, int weight) {
            this.node = node;
            this.weight = weight;
        }
    }

    public int spanningTree(int V,
                            List<List<Pair>> graph) {

        boolean[] visited = new boolean[V];

        PriorityQueue<Pair> pq =
            new PriorityQueue<>((a, b) -> a.weight - b.weight);

        pq.offer(new Pair(0, 0));

        int mstWeight = 0;

        while (!pq.isEmpty()) {
            Pair cur = pq.poll();

            int node = cur.node;
            int wt = cur.weight;

            if (visited[node]) continue;

            visited[node] = true;
            mstWeight += wt;

            for (Pair nei : graph.get(node)) {
                if (!visited[nei.node]) {
                    pq.offer(new Pair(nei.node, nei.weight));
                }
            }
        }

        return mstWeight;
    }
}
```

---

# Kruskal's Algorithm

## Idea
Kruskal’s Algorithm sorts all edges by weight.

It picks edges greedily while avoiding cycles using Disjoint Set Union (DSU).

## Time Complexity
- O(E log E)

## Java Code

```java
import java.util.*;

class Kruskal {

    static class Edge {
        int u;
        int v;
        int wt;

        Edge(int u, int v, int wt) {
            this.u = u;
            this.v = v;
            this.wt = wt;
        }
    }

    class DSU {
        int[] parent;
        int[] rank;

        DSU(int n) {
            parent = new int[n];
            rank = new int[n];

            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        int find(int x) {
            if (parent[x] == x) return x;
            return parent[x] = find(parent[x]);
        }

        boolean union(int a, int b) {
            int pa = find(a);
            int pb = find(b);

            if (pa == pb) return false;

            if (rank[pa] < rank[pb]) {
                parent[pa] = pb;
            } else if (rank[pb] < rank[pa]) {
                parent[pb] = pa;
            } else {
                parent[pb] = pa;
                rank[pa]++;
            }

            return true;
        }
    }

    public int mst(int V, List<Edge> edges) {
        Collections.sort(edges, (a, b) -> a.wt - b.wt);

        DSU dsu = new DSU(V);

        int mstWeight = 0;

        for (Edge e : edges) {
            if (dsu.union(e.u, e.v)) {
                mstWeight += e.wt;
            }
        }

        return mstWeight;
    }
}
```

## Key Points
- Prim’s uses Priority Queue.
- Kruskal’s uses DSU.
- Both are greedy algorithms.
- MST exists only for connected graphs.
