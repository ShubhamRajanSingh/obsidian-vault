---
tags:
  - dsa
  - graphs
  - trees
  - bfs
  - dfs
  - leetcode
leetcode_id: 582
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Kill Process

## Problem Link
- https://leetcode.com/problems/kill-process/

## Intuition
Processes form a parent-child tree.
Killing a process removes its entire subtree.

## Approach
1. Build adjacency list.
2. DFS/BFS from kill node.
3. Collect all reachable processes.

## Java Solution
```java
class Solution {
    public List<Integer> killProcess(List<Integer> pid,
                                     List<Integer> ppid,
                                     int kill) {

        Map<Integer, List<Integer>> graph = new HashMap<>();

        for (int i = 0; i < pid.size(); i++) {
            graph.computeIfAbsent(ppid.get(i),
                k -> new ArrayList<>()).add(pid.get(i));
        }

        List<Integer> answer = new ArrayList<>();

        dfs(kill, graph, answer);

        return answer;
    }

    private void dfs(int node,
                     Map<Integer, List<Integer>> graph,
                     List<Integer> answer) {

        answer.add(node);

        for (int child : graph.getOrDefault(node,
                                            new ArrayList<>())) {

            dfs(child, graph, answer);
        }
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Tree/graph traversal.
- Subtree collection pattern.
