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
  - Course Schedule II
  - LeetCode 210
difficulty: Medium
topic: Graph / Topological Sort
pattern: Kahn's BFS topological ordering
leetcode_id: 210
date: 2026-05-19
---

# Course Schedule II

## 📌 Problem Statement

> Return the ordering of courses you should take to finish all courses. If impossible, return an empty array.

**LeetCode #210** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] indegree = new int[numCourses];
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) adj.add(new ArrayList<>());

        for (int[] p : prerequisites) { adj.get(p[1]).add(p[0]); indegree[p[0]]++; }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) if (indegree[i] == 0) q.offer(i);

        int[] order = new int[numCourses];
        int idx = 0;
        while (!q.isEmpty()) {
            int node = q.poll();
            order[idx++] = node;
            for (int next : adj.get(node)) if (--indegree[next] == 0) q.offer(next);
        }

        return idx == numCourses ? order : new int[]{};
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(V + E) |
| Space | O(V + E) |
