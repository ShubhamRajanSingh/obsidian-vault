---
tags:
  - dsa
  - binary-tree
  - dfs
  - recursion
  - leetcode
leetcode_id: 865
complexity:
  time: O(n)
  space: O(h)
status: solved
language: java
---

# Smallest Subtree with all the Deepest Nodes

## Problem Link
- https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/

## Intuition
The answer is the lowest common ancestor of deepest leaves.
DFS returns both depth and subtree root.

## Approach
1. DFS returns:
   - deepest depth
   - candidate subtree root
2. Compare left/right depths.
3. Return deeper subtree or current node.

## Java Solution
```java
class Solution {

    class Pair {
        TreeNode node;
        int depth;

        Pair(TreeNode node, int depth) {
            this.node = node;
            this.depth = depth;
        }
    }

    public TreeNode subtreeWithAllDeepest(TreeNode root) {
        return dfs(root).node;
    }

    private Pair dfs(TreeNode node) {
        if (node == null) {
            return new Pair(null, 0);
        }

        Pair left = dfs(node.left);
        Pair right = dfs(node.right);

        if (left.depth > right.depth) {
            return new Pair(left.node, left.depth + 1);
        }

        if (right.depth > left.depth) {
            return new Pair(right.node, right.depth + 1);
        }

        return new Pair(node, left.depth + 1);
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(h)

## Key Takeaways
- Bottom-up DFS aggregation.
- LCA-style reasoning.
