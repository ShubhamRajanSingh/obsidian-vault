# Floyd Warshall Algorithm

## Tags
- dsa
- graphs
- shortest-path
- dynamic-programming
- matrix

## Idea
Floyd Warshall Algorithm finds the shortest paths between all pairs of vertices.

It works using Dynamic Programming.

The algorithm tries every node as an intermediate node.

## Time Complexity
- O(V^3)

## Java Code

```java
import java.util.*;

class FloydWarshall {

    public void shortestDistance(int[][] dist) {
        int V = dist.length;

        for (int via = 0; via < V; via++) {
            for (int i = 0; i < V; i++) {
                for (int j = 0; j < V; j++) {

                    if (dist[i][via] == (int)1e8 ||
                        dist[via][j] == (int)1e8) {
                        continue;
                    }

                    dist[i][j] = Math.min(
                        dist[i][j],
                        dist[i][via] + dist[via][j]
                    );
                }
            }
        }
    }
}
```

## Key Points
- Finds shortest paths between all pairs.
- Works with negative edges.
- Cannot handle negative cycles properly.
- Uses adjacency matrix representation.
