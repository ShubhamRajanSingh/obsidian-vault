---
tags:
  - dsa
  - binary-tree
  - dfs
  - traversal
  - leetcode
leetcode_id: 545
complexity:
  time: O(n)
  space: O(h)
status: solved
language: java
---

# Boundary of Binary Tree

## Problem Link
- https://leetcode.com/problems/boundary-of-binary-tree/

## Intuition
Boundary traversal consists of:
- left boundary
- leaves
- right boundary reversed

## Approach
1. Add root.
2. Traverse left boundary.
3. Collect leaf nodes.
4. Traverse right boundary in reverse.

## Java Solution
```java
class Solution {

    List<Integer> answer = new ArrayList<>();

    public List<Integer> boundaryOfBinaryTree(TreeNode root) {
        if (root == null) {
            return answer;
        }

        answer.add(root.val);

        leftBoundary(root.left);
        leaves(root.left);
        leaves(root.right);
        rightBoundary(root.right);

        return answer;
    }

    private boolean isLeaf(TreeNode node) {
        return node.left == null && node.right == null;
    }

    private void leftBoundary(TreeNode node) {
        while (node != null) {
            if (!isLeaf(node)) {
                answer.add(node.val);
            }

            node = (node.left != null)
                ? node.left
                : node.right;
        }
    }

    private void rightBoundary(TreeNode node) {
        Stack<Integer> stack = new Stack<>();

        while (node != null) {
            if (!isLeaf(node)) {
                stack.push(node.val);
            }

            node = (node.right != null)
                ? node.right
                : node.left;
        }

        while (!stack.isEmpty()) {
            answer.add(stack.pop());
        }
    }

    private void leaves(TreeNode node) {
        if (node == null) {
            return;
        }

        if (isLeaf(node)) {
            answer.add(node.val);
            return;
        }

        leaves(node.left);
        leaves(node.right);
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(h)

## Key Takeaways
- Specialized tree traversal.
- Boundary decomposition technique.
