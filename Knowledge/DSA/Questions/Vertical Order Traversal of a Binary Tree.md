---
tags:
  - dsa
  - binary-tree
  - bfs
  - sorting
  - hashmap
  - leetcode
leetcode_id: 987
complexity:
  time: O(n log n)
  space: O(n)
status: solved
language: java
---

# Vertical Order Traversal of a Binary Tree

## Problem Link
- https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/

## Intuition
Store nodes grouped by vertical column and row.
Sorting ensures proper traversal order.

## Approach
1. DFS traversal with:
   - row
   - column
2. Store tuples.
3. Sort by:
   - column
   - row
   - value
4. Build final answer.

## Java Solution
```java
class Solution {

    List<int[]> nodes = new ArrayList<>();

    public List<List<Integer>> verticalTraversal(TreeNode root) {
        dfs(root, 0, 0);

        Collections.sort(nodes, (a, b) -> {
            if (a[1] != b[1]) {
                return a[1] - b[1];
            }

            if (a[0] != b[0]) {
                return a[0] - b[0];
            }

            return a[2] - b[2];
        });

        List<List<Integer>> answer = new ArrayList<>();

        int prevCol = Integer.MIN_VALUE;

        for (int[] node : nodes) {
            int col = node[1];

            if (col != prevCol) {
                answer.add(new ArrayList<>());
                prevCol = col;
            }

            answer.get(answer.size() - 1).add(node[2]);
        }

        return answer;
    }

    private void dfs(TreeNode node,
                     int row,
                     int col) {

        if (node == null) {
            return;
        }

        nodes.add(new int[]{row, col, node.val});

        dfs(node.left, row + 1, col - 1);
        dfs(node.right, row + 1, col + 1);
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(n)

## Key Takeaways
- Coordinate-based tree traversal.
- Multi-key sorting.
