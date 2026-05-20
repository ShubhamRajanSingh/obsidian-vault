---
tags:
  - dsa
  - binary-tree
  - dfs
  - recursion
  - leetcode
leetcode_id: 687
complexity:
  time: O(n)
  space: O(h)
status: solved
language: java
---

# Longest Univalue Path

## Problem Link
- https://leetcode.com/problems/longest-univalue-path/

## Intuition
For every node, compute the longest same-value path extending from left and right.
Combine both sides to update answer.

## Approach
1. DFS traversal.
2. Compute left/right path lengths.
3. Extend only if child value matches current node.
4. Update global answer.

## Java Solution
```java
class Solution {
    int answer = 0;

    public int longestUnivaluePath(TreeNode root) {
        dfs(root);
        return answer;
    }

    private int dfs(TreeNode node) {
        if (node == null) {
            return 0;
        }

        int left = dfs(node.left);
        int right = dfs(node.right);

        int leftPath = 0;
        int rightPath = 0;

        if (node.left != null && node.left.val == node.val) {
            leftPath = left + 1;
        }

        if (node.right != null && node.right.val == node.val) {
            rightPath = right + 1;
        }

        answer = Math.max(answer, leftPath + rightPath);

        return Math.max(leftPath, rightPath);
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(h)

## Key Takeaways
- Tree diameter-style DFS.
- Combine left and right contributions.
