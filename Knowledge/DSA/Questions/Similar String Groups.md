---
tags:
  - dsa
  - union-find
  - strings
  - graphs
  - leetcode
leetcode_id: 839
complexity:
  time: O(n^2 * m)
  space: O(n)
status: solved
language: java
---

# Similar String Groups

## Problem Link
- https://leetcode.com/problems/similar-string-groups/

## Intuition
Two strings belong to same group if they differ at at most two positions.
Union-Find efficiently merges groups.

## Approach
1. Compare every pair.
2. Check similarity.
3. Union similar strings.
4. Count distinct parents.

## Java Solution
```java
class Solution {
    public int numSimilarGroups(String[] strs) {
        int n = strs.length;

        DSU dsu = new DSU(n);

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (similar(strs[i], strs[j])) {
                    dsu.union(i, j);
                }
            }
        }

        return dsu.count;
    }

    private boolean similar(String a, String b) {
        int diff = 0;

        for (int i = 0; i < a.length(); i++) {
            if (a.charAt(i) != b.charAt(i)) {
                diff++;
            }
        }

        return diff == 0 || diff == 2;
    }

    class DSU {
        int[] parent;
        int count;

        DSU(int n) {
            parent = new int[n];
            count = n;

            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]);
            }

            return parent[x];
        }

        void union(int a, int b) {
            int pa = find(a);
            int pb = find(b);

            if (pa != pb) {
                parent[pa] = pb;
                count--;
            }
        }
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^2 * m)
- Space Complexity: O(n)

## Key Takeaways
- String similarity graph.
- Union-Find connected components.
