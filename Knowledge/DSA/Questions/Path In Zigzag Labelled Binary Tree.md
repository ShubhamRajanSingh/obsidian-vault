---
tags:
  - dsa
  - binary-tree
  - math
  - simulation
  - leetcode
leetcode_id: 1104
complexity:
  time: O(log n)
  space: O(log n)
status: solved
language: java
---

# Path In Zigzag Labelled Binary Tree

## Problem Link
- https://leetcode.com/problems/path-in-zigzag-labelled-binary-tree/

## Intuition
Levels alternate between normal and reversed labeling.
Mirror transformation converts zigzag labels into normal tree positions.

## Approach
1. Determine current level.
2. Compute mirrored parent.
3. Move upward until root.
4. Reverse collected path.

## Java Solution
```java
class Solution {
    public List<Integer> pathInZigZagTree(int label) {
        List<Integer> path = new ArrayList<>();

        int level = 0;
        int temp = label;

        while (temp > 1) {
            temp /= 2;
            level++;
        }

        while (label >= 1) {
            path.add(label);

            int start = 1 << level;
            int end = (1 << (level + 1)) - 1;

            label = (start + end - label) / 2;
            level--;
        }

        Collections.reverse(path);

        return path;
    }
}
```

## Complexity Analysis
- Time Complexity: O(log n)
- Space Complexity: O(log n)

## Key Takeaways
- Zigzag label transformation.
- Binary tree arithmetic reasoning.
