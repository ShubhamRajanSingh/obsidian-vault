---
tags:
  - dsa
  - trie
  - strings
  - hashmap
  - leetcode
leetcode_id: 648
complexity:
  time: O(n * l)
  space: O(total characters)
status: solved
language: java
---

# Replace Words

## Problem Link
- https://leetcode.com/problems/replace-words/

## Intuition
Trie helps efficiently locate the shortest matching root word.

## Approach
1. Insert all dictionary roots into trie.
2. For each word:
   - Traverse trie.
   - Stop at first valid root.
3. Replace word if root exists.

## Java Solution
```java
class Solution {

    class TrieNode {
        TrieNode[] children = new TrieNode[26];
        boolean isWord;
    }

    TrieNode root = new TrieNode();

    public String replaceWords(List<String> dictionary, String sentence) {
        for (String word : dictionary) {
            insert(word);
        }

        StringBuilder result = new StringBuilder();

        for (String word : sentence.split(" ")) {
            result.append(search(word)).append(" ");
        }

        return result.toString().trim();
    }

    private void insert(String word) {
        TrieNode node = root;

        for (char ch : word.toCharArray()) {
            int idx = ch - 'a';

            if (node.children[idx] == null) {
                node.children[idx] = new TrieNode();
            }

            node = node.children[idx];
        }

        node.isWord = true;
    }

    private String search(String word) {
        TrieNode node = root;
        StringBuilder prefix = new StringBuilder();

        for (char ch : word.toCharArray()) {
            int idx = ch - 'a';

            if (node.children[idx] == null) {
                return word;
            }

            prefix.append(ch);
            node = node.children[idx];

            if (node.isWord) {
                return prefix.toString();
            }
        }

        return word;
    }
}
```

## Complexity Analysis
- Time Complexity: O(total characters)
- Space Complexity: O(total characters)

## Key Takeaways
- Trie prefix matching.
- Shortest root optimization.
