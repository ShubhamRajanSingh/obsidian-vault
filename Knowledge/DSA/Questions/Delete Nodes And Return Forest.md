---
tags:
  - dsa
  - binary-tree
  - dfs
  - recursion
  - leetcode
leetcode_id: 1110
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Delete Nodes And Return Forest

## Problem Link
- https://leetcode.com/problems/delete-nodes-and-return-forest/

## Intuition
DFS recursively deletes nodes.
Children of deleted nodes become new roots.

## Approach
1. Store deleted values in set.
2. DFS traversal.
3. If current node deleted:
   - add children as new roots
4. Return surviving subtree.

## Java Solution
```java
class Solution {

    Set<Integer> deleted = new HashSet<>();
    List<TreeNode> forest = new ArrayList<>();

    public List<TreeNode> delNodes(TreeNode root,
                                   int[] to_delete) {

        for (int val : to_delete) {
            deleted.add(val);
        }

        root = dfs(root);

        if (root != null) {
            forest.add(root);
        }

        return forest;
    }

    private TreeNode dfs(TreeNode node) {
        if (node == null) {
            return null;
        }

        node.left = dfs(node.left);
        node.right = dfs(node.right);

        if (deleted.contains(node.val)) {
            if (node.left != null) {
                forest.add(node.left);
            }

            if (node.right != null) {
                forest.add(node.right);
            }

            return null;
        }

        return node;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Recursive subtree deletion.
- Forest reconstruction.
