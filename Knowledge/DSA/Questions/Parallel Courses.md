---
tags:
  - dsa
  - graphs
  - topological-sort
  - bfs
  - leetcode
leetcode_id: 1136
complexity:
  time: O(V + E)
  space: O(V + E)
status: solved
language: java
---

# Parallel Courses

## Problem Link
- https://leetcode.com/problems/parallel-courses/

## Intuition
Courses form a directed graph.
Topological sorting determines semester ordering.

## Approach
1. Build graph and indegree array.
2. Add indegree-0 nodes to queue.
3. BFS level traversal.
4. Count semesters.
5. Detect cycle if courses remain unfinished.

## Java Solution
```java
class Solution {
    public int minimumSemesters(int n, int[][] relations) {
        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }

        int[] indegree = new int[n + 1];

        for (int[] edge : relations) {
            graph.get(edge[0]).add(edge[1]);
            indegree[edge[1]]++;
        }

        Queue<Integer> queue = new LinkedList<>();

        for (int i = 1; i <= n; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }

        int semesters = 0;
        int completed = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();
            semesters++;

            for (int i = 0; i < size; i++) {
                int node = queue.poll();
                completed++;

                for (int next : graph.get(node)) {
                    indegree[next]--;

                    if (indegree[next] == 0) {
                        queue.offer(next);
                    }
                }
            }
        }

        return completed == n ? semesters : -1;
    }
}
```

## Complexity Analysis
- Time Complexity: O(V + E)
- Space Complexity: O(V + E)

## Key Takeaways
- Kahn's Algorithm.
- BFS topological ordering.
