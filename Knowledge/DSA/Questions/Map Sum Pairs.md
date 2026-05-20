---
tags:
  - dsa
  - trie
  - hashmap
  - design
  - leetcode
leetcode_id: 677
complexity:
  time: O(n)
  space: O(total characters)
status: solved
language: java
---

# Map Sum Pairs

## Problem Link
- https://leetcode.com/problems/map-sum-pairs/

## Intuition
Trie efficiently stores prefixes.
Each node maintains cumulative prefix score.

## Approach
1. Maintain hashmap for previous values.
2. Compute delta during updates.
3. Propagate delta through trie.
4. Prefix query returns stored score.

## Java Solution
```java
class MapSum {

    class TrieNode {
        TrieNode[] children = new TrieNode[26];
        int score;
    }

    TrieNode root;
    Map<String, Integer> map;

    public MapSum() {
        root = new TrieNode();
        map = new HashMap<>();
    }

    public void insert(String key, int val) {
        int delta = val - map.getOrDefault(key, 0);
        map.put(key, val);

        TrieNode node = root;
        node.score += delta;

        for (char ch : key.toCharArray()) {
            int idx = ch - 'a';

            if (node.children[idx] == null) {
                node.children[idx] = new TrieNode();
            }

            node = node.children[idx];
            node.score += delta;
        }
    }

    public int sum(String prefix) {
        TrieNode node = root;

        for (char ch : prefix.toCharArray()) {
            int idx = ch - 'a';

            if (node.children[idx] == null) {
                return 0;
            }

            node = node.children[idx];
        }

        return node.score;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(total characters)

## Key Takeaways
- Trie prefix aggregation.
- Delta update optimization.
