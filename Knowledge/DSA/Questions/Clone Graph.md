---
tags:
  - dsa
  - graph
  - bfs
  - dfs
  - medium
  - leetcode
  - striver
aliases:
  - Clone Graph
  - LeetCode 133
difficulty: Medium
topic: Graph / BFS / DFS
pattern: DFS/BFS with visited map
leetcode_id: 133
date: 2026-05-19
---

# Clone Graph

## 📌 Problem Statement

> Given a reference to a node in a connected undirected graph, return a deep copy (clone) of the graph.

**LeetCode #133** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (DFS)

```java
class Solution {
    Map<Node, Node> visited = new HashMap<>();

    public Node cloneGraph(Node node) {
        if (node == null) return null;
        if (visited.containsKey(node)) return visited.get(node);

        Node clone = new Node(node.val);
        visited.put(node, clone);

        for (Node neighbor : node.neighbors) {
            clone.neighbors.add(cloneGraph(neighbor));
        }

        return clone;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(V + E) |
| Space | O(V) |
