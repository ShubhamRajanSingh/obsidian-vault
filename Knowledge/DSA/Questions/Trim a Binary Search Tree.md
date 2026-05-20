---
tags:
  - dsa
  - binary-search-tree
  - trees
  - recursion
  - leetcode
leetcode_id: 669
complexity:
  time: O(n)
  space: O(h)
status: solved
language: java
---

# Trim a Binary Search Tree

## Problem Link
- https://leetcode.com/problems/trim-a-binary-search-tree/

## Intuition
BST property allows pruning entire subtrees efficiently.

## Approach
1. If node value < low:
   - discard left subtree
2. If node value > high:
   - discard right subtree
3. Recursively trim children.

## Java Solution
```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }

        if (root.val < low) {
            return trimBST(root.right, low, high);
        }

        if (root.val > high) {
            return trimBST(root.left, low, high);
        }

        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);

        return root;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(h)

## Key Takeaways
- BST pruning optimization.
- Recursive tree trimming pattern.
