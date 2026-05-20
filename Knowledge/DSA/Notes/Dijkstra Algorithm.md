# Dijkstra Algorithm

## Tags
- dsa
- graphs
- shortest-path
- priority-queue
- greedy

## Idea
Dijkstra’s Algorithm is used to find the shortest path from a source node to all other nodes in a weighted graph with non-negative weights.

It uses a Min Heap / Priority Queue to always process the node with the smallest known distance.

## Time Complexity
- Using Priority Queue: O(E log V)

## Java Code

```java
import java.util.*;

class Dijkstra {

    static class Pair {
        int node;
        int dist;

        Pair(int node, int dist) {
            this.node = node;
            this.dist = dist;
        }
    }

    public int[] shortestPath(int V,
                              List<List<Pair>> graph,
                              int src) {

        PriorityQueue<Pair> pq =
            new PriorityQueue<>((a, b) -> a.dist - b.dist);

        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);

        dist[src] = 0;
        pq.offer(new Pair(src, 0));

        while (!pq.isEmpty()) {
            Pair cur = pq.poll();

            int node = cur.node;
            int d = cur.dist;

            if (d > dist[node]) continue;

            for (Pair nei : graph.get(node)) {
                int next = nei.node;
                int weight = nei.dist;

                if (dist[node] + weight < dist[next]) {
                    dist[next] = dist[node] + weight;
                    pq.offer(new Pair(next, dist[next]));
                }
            }
        }

        return dist;
    }
}
```

## Key Points
- Works only for non-negative weights.
- Greedy algorithm.
- Priority Queue optimizes shortest node selection.
- Commonly asked in interviews.
