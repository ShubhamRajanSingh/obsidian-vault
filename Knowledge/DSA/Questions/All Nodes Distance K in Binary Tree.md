---
tags:
  - dsa
  - binary-tree
  - bfs
  - graphs
  - dfs
  - leetcode
leetcode_id: 863
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# All Nodes Distance K in Binary Tree

## Problem Link
- https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/

## Intuition
Convert tree into an undirected graph using parent pointers.
Then perform BFS starting from target node.

## Approach
1. Build parent map using DFS.
2. Start BFS from target.
3. Traverse:
   - left child
   - right child
   - parent
4. Collect nodes at distance k.

## Java Solution
```java
class Solution {
    Map<TreeNode, TreeNode> parentMap = new HashMap<>();

    public List<Integer> distanceK(TreeNode root,
                                   TreeNode target,
                                   int k) {

        buildParent(root, null);

        Queue<TreeNode> queue = new LinkedList<>();
        Set<TreeNode> visited = new HashSet<>();

        queue.offer(target);
        visited.add(target);

        int distance = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();

            if (distance == k) {
                List<Integer> answer = new ArrayList<>();

                for (TreeNode node : queue) {
                    answer.add(node.val);
                }

                return answer;
            }

            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();

                if (node.left != null && visited.add(node.left)) {
                    queue.offer(node.left);
                }

                if (node.right != null && visited.add(node.right)) {
                    queue.offer(node.right);
                }

                TreeNode parent = parentMap.get(node);

                if (parent != null && visited.add(parent)) {
                    queue.offer(parent);
                }
            }

            distance++;
        }

        return new ArrayList<>();
    }

    private void buildParent(TreeNode node, TreeNode parent) {
        if (node == null) {
            return;
        }

        parentMap.put(node, parent);

        buildParent(node.left, node);
        buildParent(node.right, node);
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Tree to graph conversion.
- BFS level traversal.
