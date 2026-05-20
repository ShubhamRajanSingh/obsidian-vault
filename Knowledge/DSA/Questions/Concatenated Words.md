---
tags:
  - dsa
  - trie
  - dynamic-programming
  - strings
  - dfs
  - leetcode
leetcode_id: 472
complexity:
  time: O(n * l^2)
  space: O(total characters)
status: solved
language: java
---

# Concatenated Words

## Problem Link
- https://leetcode.com/problems/concatenated-words/

## Intuition
A concatenated word can be formed using smaller dictionary words.
DP checks valid segmentations.

## Approach
1. Store all words in set.
2. For each word:
   - temporarily remove it
   - run word break DP
3. Add valid concatenated words.

## Java Solution
```java
class Solution {
    public List<String> findAllConcatenatedWordsInADict(
            String[] words) {

        Set<String> set = new HashSet<>(Arrays.asList(words));

        List<String> answer = new ArrayList<>();

        for (String word : words) {
            set.remove(word);

            if (canForm(word, set)) {
                answer.add(word);
            }

            set.add(word);
        }

        return answer;
    }

    private boolean canForm(String word,
                            Set<String> set) {

        if (set.isEmpty()) {
            return false;
        }

        boolean[] dp = new boolean[word.length() + 1];
        dp[0] = true;

        for (int i = 1; i <= word.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] &&
                    set.contains(word.substring(j, i))) {

                    dp[i] = true;
                    break;
                }
            }
        }

        return dp[word.length()];
    }
}
```

## Complexity Analysis
- Time Complexity: O(n * l^2)
- Space Complexity: O(l)

## Key Takeaways
- Word break DP reuse.
- Dictionary segmentation technique.
