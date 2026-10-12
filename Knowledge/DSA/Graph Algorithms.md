# Graph Algorithms

#tags: #dsa #graph #index

Index of graph algorithm notes and graph problems in this vault. Topic notes live in `Notes/`, problems in `Questions/`.

---

## Algorithm Notes

### Shortest Paths
- [[Dijkstra Algorithm]] — single-source shortest path on weighted graphs with non-negative edges, using a priority queue.
- [[Bellman Ford Algorithm]] — single-source shortest path that also handles negative edge weights.
- [[Floyd Warshall Algorithm]] — all-pairs shortest paths via DP over intermediate vertices.

### Minimum Spanning Tree
- [[Minimum Spanning Tree Algorithms]] — Kruskal (union-find) and Prim for connecting all vertices at minimum total weight.

### Ordering
- [[Topological Sort]] — indegree/queue ordering of a DAG, detects cycles.

---

## Problems

### Traversal and Connectivity (BFS / DFS)
- [[Find if Path Exists in Graph]] — build adjacency list and check source-to-destination reachability.
- [[Keys and Rooms]] — DFS/BFS to see if every room can be reached from room 0.
- [[All Paths From Source to Target]] — enumerate all paths in a DAG with DFS backtracking.
- [[Clone Graph]] — deep copy a graph using a visited map.
- [[Number of Provinces]] — count connected components in an adjacency matrix.
- [[Count the Number of Complete Components]] — count connected components that are complete subgraphs.
- [[Minimize Malware Spread]] — pick the node whose removal minimizes infection, based on component sizes.
- [[Kill Process]] — BFS/DFS over a parent-to-children process tree.
- [[All Nodes Distance K in Binary Tree]] — treat the tree as an undirected graph and BFS from the target.
- [[Find Center of Star Graph]] — the center is the node common to any two edges.
- [[Celebrity Problem]] — find the node known by everyone who knows no one, via an implicit directed graph.

### Grid as Graph
- [[Number of Islands]] — count connected land components in a grid.
- [[LC200 Island Counter]] — DFS flood-fill solution to Number of Islands.
- [[Making A Large Island]] — label islands, then evaluate flipping one water cell to merge them.
- [[Path With Maximum Gold]] — DFS with backtracking from every gold cell.
- [[Rotting Oranges]] — multi-source BFS counting minutes until all oranges rot.
- [[Rotten Oranges]] — multi-source BFS write-up of the same problem.
- [[Rotting Oranges BFS Variant]] — queue-based multi-source BFS implementation.
- [[01 Matrix]] — multi-source BFS from zeros for distance to nearest 0.
- [[As Far from Land as Possible]] — multi-source BFS from land to find the farthest water cell.
- [[Shortest Path in Binary Matrix]] — BFS over 8-directional moves for the shortest clear path.
- [[Snakes and Ladders]] — BFS over board squares with snake/ladder jumps.
- [[Swim in Rising Water]] — minimize the maximum cell value along a path from corner to corner.
- [[Path With Maximum Minimum Value]] — maximize the minimum value along a path through the grid.

### State-Space and Shortest-Path BFS
- [[Open the Lock]] — BFS over lock combinations while avoiding deadends.
- [[Word Ladder II]] — BFS for shortest transformations, then backtrack all shortest sequences.
- [[Shortest Path Visiting All Nodes]] — BFS over (node, visited-mask) states.
- [[Cat and Mouse]] — game-theory search over graph states with BFS and DP.
- [[Similar String Groups]] — group strings into connected components by similarity.

### Weighted Shortest Paths
- [[Network Delay Time]] — Dijkstra (or Bellman-Ford) for the time a signal takes to reach all nodes.
- [[Cheapest Flights Within K Stops]] — shortest path with at most K edges (Bellman-Ford style or BFS).
- [[Number of Ways to Arrive at Destination]] — Dijkstra while counting the number of shortest paths.

### Topological Sort and Cycles
- [[Course Schedule]] — detect a cycle in the prerequisite graph.
- [[Course Schedule II]] — produce a valid course order via topological sort.
- [[Parallel Courses]] — topological levels to find the minimum number of semesters.
- [[Find Eventual Safe States]] — reverse graph topological sort to find nodes that never reach a cycle.

### Bipartite Graphs
- [[Is Graph Bipartite]] — two-color the graph with BFS/DFS.
- [[Bipartite Graph Check]] — bipartite check via two-coloring.
- [[Possible Bipartition]] — split people into two groups with no dislike inside a group.

### Union-Find and Spanning Trees
- [[Accounts Merge]] — union accounts that share an email.
- [[Redundant Connection]] — find the edge that closes a cycle in an undirected graph.
- [[Redundant Connection II]] — the same in a directed graph, handling nodes with two parents.
- [[Satisfiability of Equality Equations]] — union `==` variables, then verify `!=` constraints.
- [[Couples Holding Hands]] — union couples to count the swaps needed.
- [[Optimize Water Distribution in a Village]] — MST with a virtual well node.

---

## Related
- [[DSA Revision Dashboard]]
