---
tags:
  - dsa
  - binary-tree
  - stack
  - dfs
  - parsing
  - leetcode
leetcode_id: 1028
complexity:
  time: O(n)
  space: O(h)
status: solved
language: java
---

# Recover a Tree From Preorder Traversal

## Problem Link
- https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/

## Intuition
Depth is represented using dashes.
Stack maintains current path from root.

## Approach
1. Parse depth and node value.
2. Pop stack until correct parent depth found.
3. Attach node as left/right child.
4. Push current node.

## Java Solution
```java
class Solution {
    public TreeNode recoverFromPreorder(String traversal) {
        Stack<TreeNode> stack = new Stack<>();

        int i = 0;

        while (i < traversal.length()) {
            int depth = 0;

            while (traversal.charAt(i) == '-') {
                depth++;
                i++;
            }

            int value = 0;

            while (i < traversal.length() &&
                   Character.isDigit(traversal.charAt(i))) {

                value = value * 10 +
                        (traversal.charAt(i) - '0');

                i++;
            }

            TreeNode node = new TreeNode(value);

            while (stack.size() > depth) {
                stack.pop();
            }

            if (!stack.isEmpty()) {
                TreeNode parent = stack.peek();

                if (parent.left == null) {
                    parent.left = node;
                } else {
                    parent.right = node;
                }
            }

            stack.push(node);
        }

        while (stack.size() > 1) {
            stack.pop();
        }

        return stack.peek();
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(h)

## Key Takeaways
- Tree parsing using stack.
- Depth-controlled reconstruction.
