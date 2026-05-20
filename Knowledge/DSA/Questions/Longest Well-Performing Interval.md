---
tags:
  - dsa
  - prefix-sum
  - hashmap
  - arrays
  - leetcode
leetcode_id: 1124
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Longest Well-Performing Interval

## Problem Link
- https://leetcode.com/problems/longest-well-performing-interval/

## Intuition
Convert hours into +1/-1 scores.
We need longest subarray with positive prefix difference.

## Approach
1. Treat:
   - >8 hours => +1
   - <=8 hours => -1
2. Maintain prefix sum.
3. Use hashmap to store earliest occurrence.
4. Find longest valid interval.

## Java Solution
```java
class Solution {
    public int longestWPI(int[] hours) {
        Map<Integer, Integer> map = new HashMap<>();

        int prefix = 0;
        int answer = 0;

        for (int i = 0; i < hours.length; i++) {
            prefix += hours[i] > 8 ? 1 : -1;

            if (prefix > 0) {
                answer = i + 1;
            } else {
                map.putIfAbsent(prefix, i);

                if (map.containsKey(prefix - 1)) {
                    answer = Math.max(
                        answer,
                        i - map.get(prefix - 1)
                    );
                }
            }
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Prefix sum transformation.
- Earliest occurrence optimization.
