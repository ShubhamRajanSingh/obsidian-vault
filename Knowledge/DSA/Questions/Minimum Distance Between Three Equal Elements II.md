---
tags:
  - dsa
  - arrays
  - hashing
  - sliding-window
  - leetcode
leetcode_id: 3741
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Minimum Distance Between Three Equal Elements II

## Problem Link
- https://leetcode.com/problems/minimum-distance-between-three-equal-elements-ii/

## Intuition
Track occurrences of every number.
Whenever we encounter the third occurrence candidate, compute distance.

## Approach
1. Store occurrence indices for every value.
2. Maintain sliding window of last three occurrences.
3. Compute minimum distance.

## Java Solution
```java
class Solution {
    public int minimumDistance(int[] nums) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        int answer = Integer.MAX_VALUE;

        for (int i = 0; i < nums.length; i++) {
            map.putIfAbsent(nums[i], new ArrayList<>());
            List<Integer> list = map.get(nums[i]);

            list.add(i);

            if (list.size() >= 3) {
                int size = list.size();
                answer = Math.min(answer,
                        list.get(size - 1) - list.get(size - 3));
            }
        }

        return answer == Integer.MAX_VALUE ? -1 : answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Frequency occurrence tracking pattern.
- Sliding occurrence window optimization.
