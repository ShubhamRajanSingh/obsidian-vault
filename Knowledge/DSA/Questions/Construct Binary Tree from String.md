---
tags:
  - dsa
  - binary-tree
  - parsing
  - recursion
  - strings
  - leetcode
leetcode_id: 536
complexity:
  time: O(n)
  space: O(h)
status: solved
language: java
---

# Construct Binary Tree from String

## Problem Link
- https://leetcode.com/problems/construct-binary-tree-from-string/

## Intuition
Parentheses naturally represent recursive subtree structure.
Recursive parsing reconstructs the tree.

## Approach
1. Parse node value.
2. Recursively build left subtree.
3. Recursively build right subtree.
4. Move parsing pointer accordingly.

## Java Solution
```java
class Solution {

    int index = 0;

    public TreeNode str2tree(String s) {
        if (s.length() == 0) {
            return null;
        }

        return build(s);
    }

    private TreeNode build(String s) {
        int sign = 1;

        if (s.charAt(index) == '-') {
            sign = -1;
            index++;
        }

        int value = 0;

        while (index < s.length() &&
               Character.isDigit(s.charAt(index))) {

            value = value * 10 +
                    (s.charAt(index) - '0');

            index++;
        }

        TreeNode node = new TreeNode(sign * value);

        if (index < s.length() && s.charAt(index) == '(') {
            index++;
            node.left = build(s);
            index++;
        }

        if (index < s.length() && s.charAt(index) == '(') {
            index++;
            node.right = build(s);
            index++;
        }

        return node;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(h)

## Key Takeaways
- Recursive descent parsing.
- Tree reconstruction from encoded strings.
