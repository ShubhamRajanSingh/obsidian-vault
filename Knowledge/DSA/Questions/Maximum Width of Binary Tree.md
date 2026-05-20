---
tags:
  - dsa
  - binary-tree
  - bfs
  - trees
  - leetcode
leetcode_id: 662
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Maximum Width of Binary Tree

## Problem Link
- https://leetcode.com/problems/maximum-width-of-binary-tree/

## Intuition
Assign virtual indices to nodes like a complete binary tree.
Width becomes:
lastIndex - firstIndex + 1

## Approach
1. BFS traversal.
2. Store node with index.
3. Normalize indices per level.
4. Track maximum width.

## Java Solution
```java
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        Queue<Pair> queue = new LinkedList<>();
        queue.offer(new Pair(root, 0L));

        int answer = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();

            long min = queue.peek().index;
            long first = 0;
            long last = 0;

            for (int i = 0; i < size; i++) {
                Pair current = queue.poll();

                long idx = current.index - min;

                if (i == 0) {
                    first = idx;
                }

                if (i == size - 1) {
                    last = idx;
                }

                if (current.node.left != null) {
                    queue.offer(new Pair(
                        current.node.left,
                        2 * idx + 1
                    ));
                }

                if (current.node.right != null) {
                    queue.offer(new Pair(
                        current.node.right,
                        2 * idx + 2
                    ));
                }
            }

            answer = Math.max(answer,
                (int)(last - first + 1));
        }

        return answer;
    }

    class Pair {
        TreeNode node;
        long index;

        Pair(TreeNode node, long index) {
            this.node = node;
            this.index = index;
        }
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Virtual indexing trick.
- BFS level processing.
