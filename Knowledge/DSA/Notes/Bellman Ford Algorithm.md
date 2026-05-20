# Bellman Ford Algorithm

## Tags
- dsa
- graphs
- shortest-path
- dynamic-programming

## Idea
Bellman Ford Algorithm finds the shortest distance from a source node to all vertices.

Unlike Dijkstra, it can handle negative edge weights.

It relaxes all edges exactly V-1 times.

## Time Complexity
- O(V * E)

## Java Code

```java
import java.util.*;

class BellmanFord {

    static class Edge {
        int src;
        int dest;
        int weight;

        Edge(int src, int dest, int weight) {
            this.src = src;
            this.dest = dest;
            this.weight = weight;
        }
    }

    public int[] shortestPath(int V,
                              List<Edge> edges,
                              int src) {

        int[] dist = new int[V];
        Arrays.fill(dist, (int)1e8);

        dist[src] = 0;

        for (int i = 1; i <= V - 1; i++) {
            for (Edge e : edges) {
                if (dist[e.src] != (int)1e8 &&
                    dist[e.src] + e.weight < dist[e.dest]) {

                    dist[e.dest] = dist[e.src] + e.weight;
                }
            }
        }

        for (Edge e : edges) {
            if (dist[e.src] != (int)1e8 &&
                dist[e.src] + e.weight < dist[e.dest]) {

                System.out.println("Negative Cycle Detected");
            }
        }

        return dist;
    }
}
```

## Key Points
- Handles negative weights.
- Detects negative cycles.
- Slower than Dijkstra.
- Useful in graph theory problems.
