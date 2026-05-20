---
tags:
  - dsa
  - graph
  - topological-sort
  - bfs
  - medium
  - leetcode
  - striver
aliases:
  - Course Schedule
  - LeetCode 207
difficulty: Medium
topic: Graph / Topological Sort
pattern: Cycle detection via BFS (Kahn's) or DFS
leetcode_id: 207
date: 2026-05-19
---

# Course Schedule

## 📌 Problem Statement

> There are `numCourses` courses labeled 0 to n-1. Given `prerequisites[i] = [a, b]` meaning you must take course `b` before `a`, return `true` if you can finish all courses (no cycle).

**LeetCode #207** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (Kahn's BFS)

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] indegree = new int[numCourses];
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());

        for (int[] p : prerequisites) { adj.get(p[1]).add(p[0]); indegree[p[0]]++; }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) if (indegree[i] == 0) q.offer(i);

        int processed = 0;
        while (!q.isEmpty()) {
            int node = q.poll(); processed++;
            for (int next : adj.get(node)) if (--indegree[next] == 0) q.offer(next);
        }

        return processed == numCourses;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(V + E) |
| Space | O(V + E) |

---

## 🔗 Related

- [[Course Schedule II]]
- [[Course Schedule III]]
