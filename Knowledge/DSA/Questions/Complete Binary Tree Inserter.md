---
tags:
  - dsa
  - binary-tree
  - bfs
  - design
  - leetcode
leetcode_id: 919
complexity:
  time: O(1)
  space: O(n)
status: solved
language: java
---

# Complete Binary Tree Inserter

## Problem Link
- https://leetcode.com/problems/complete-binary-tree-inserter/

## Intuition
A queue can maintain candidate nodes having missing children.
Insertion always happens at the first incomplete node.

## Approach
1. Level-order traverse initial tree.
2. Store incomplete nodes in queue.
3. Insert child into front node.
4. Remove node when fully occupied.

## Java Solution
```java
class CBTInserter {

    TreeNode root;
    Queue<TreeNode> deque;

    public CBTInserter(TreeNode root) {
        this.root = root;
        deque = new LinkedList<>();

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();

            if (node.left == null || node.right == null) {
                deque.offer(node);
            }

            if (node.left != null) {
                queue.offer(node.left);
            }

            if (node.right != null) {
                queue.offer(node.right);
            }
        }
    }

    public int insert(int val) {
        TreeNode parent = deque.peek();
        TreeNode node = new TreeNode(val);

        if (parent.left == null) {
            parent.left = node;
        } else {
            parent.right = node;
            deque.poll();
        }

        deque.offer(node);

        return parent.val;
    }

    public TreeNode get_root() {
        return root;
    }
}
```

## Complexity Analysis
- Time Complexity: O(1) per insert
- Space Complexity: O(n)

## Key Takeaways
- Queue-based complete tree maintenance.
- Design/data structure problem.
