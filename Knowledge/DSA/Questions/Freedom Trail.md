---
tags:
  - dsa
  - dynamic-programming
  - dfs
  - memoization
  - strings
  - leetcode
leetcode_id: 514
complexity:
  time: O(m * n^2)
  space: O(m * n)
status: solved
language: java
---

# Freedom Trail

## Problem Link
- https://leetcode.com/problems/freedom-trail/

## Intuition
For each key character, choose the optimal rotation position.
Memoized DFS avoids repeated computation.

## Approach
1. Map character positions in ring.
2. DFS(state):
   - current ring index
   - current key index
3. Try all matching positions.
4. Take minimum rotation cost.

## Java Solution
```java
class Solution {

    Map<Character, List<Integer>> map = new HashMap<>();
    Map<String, Integer> memo = new HashMap<>();

    public int findRotateSteps(String ring,
                               String key) {

        for (int i = 0; i < ring.length(); i++) {
            map.computeIfAbsent(ring.charAt(i),
                k -> new ArrayList<>()).add(i);
        }

        return dfs(ring, key, 0, 0);
    }

    private int dfs(String ring,
                    String key,
                    int ringPos,
                    int keyPos) {

        if (keyPos == key.length()) {
            return 0;
        }

        String state = ringPos + "," + keyPos;

        if (memo.containsKey(state)) {
            return memo.get(state);
        }

        int n = ring.length();
        int answer = Integer.MAX_VALUE;

        for (int next : map.get(key.charAt(keyPos))) {
            int diff = Math.abs(next - ringPos);
            int steps = Math.min(diff, n - diff);

            answer = Math.min(answer,
                steps + 1 +
                dfs(ring, key, next, keyPos + 1));
        }

        memo.put(state, answer);
        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(m * n^2)
- Space Complexity: O(m * n)

## Key Takeaways
- Circular DP optimization.
- Memoized graph traversal.
