---
tags:
  - dsa
  - union-find
  - graphs
  - strings
  - leetcode
leetcode_id: 990
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Satisfiability of Equality Equations

## Problem Link
- https://leetcode.com/problems/satisfiability-of-equality-equations/

## Intuition
Equal variables belong to same connected component.
Union-Find efficiently models equivalence relationships.

## Approach
1. Union all equality equations.
2. Validate inequality equations.
3. If conflicting variables belong to same set:
   return false.

## Java Solution
```java
class Solution {

    int[] parent = new int[26];

    public boolean equationsPossible(String[] equations) {
        for (int i = 0; i < 26; i++) {
            parent[i] = i;
        }

        for (String eq : equations) {
            if (eq.charAt(1) == '=') {
                int a = eq.charAt(0) - 'a';
                int b = eq.charAt(3) - 'a';

                union(a, b);
            }
        }

        for (String eq : equations) {
            if (eq.charAt(1) == '!') {
                int a = eq.charAt(0) - 'a';
                int b = eq.charAt(3) - 'a';

                if (find(a) == find(b)) {
                    return false;
                }
            }
        }

        return true;
    }

    private int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }

        return parent[x];
    }

    private void union(int a, int b) {
        parent[find(a)] = find(b);
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Union-Find equivalence classes.
- Constraint validation.
