---
tags:
  - dsa
  - binary-tree
  - bfs
  - trees
  - leetcode
leetcode_id: 1161
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Maximum Level Sum of a Binary Tree

## Problem Link
- https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/

## Intuition
Level-order traversal naturally computes sums level by level.

## Approach
1. BFS traversal.
2. Compute sum for each level.
3. Track maximum level sum.

## Java Solution
```java
class Solution {
    public int maxLevelSum(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        int bestLevel = 1;
        int level = 1;
        int bestSum = Integer.MIN_VALUE;

        while (!queue.isEmpty()) {
            int size = queue.size();
            int sum = 0;

            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                sum += node.val;

                if (node.left != null) {
                    queue.offer(node.left);
                }

                if (node.right != null) {
                    queue.offer(node.right);
                }
            }

            if (sum > bestSum) {
                bestSum = sum;
                bestLevel = level;
            }

            level++;
        }

        return bestLevel;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Level-order traversal.
- Aggregation per BFS layer.
