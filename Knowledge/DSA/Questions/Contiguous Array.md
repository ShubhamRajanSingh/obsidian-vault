---
tags:
  - dsa
  - prefix-sum
  - hashmap
  - arrays
  - leetcode
leetcode_id: 525
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Contiguous Array

## Problem Link
- https://leetcode.com/problems/contiguous-array/

## Intuition
Treat:
- 0 as -1
- 1 as +1

Equal prefix sums imply equal number of 0s and 1s.

## Approach
1. Maintain running prefix sum.
2. Store earliest occurrence of each prefix.
3. If prefix repeats:
   subarray between them is balanced.
4. Track maximum length.

## Java Solution
```java
class Solution {
    public int findMaxLength(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);

        int prefix = 0;
        int answer = 0;

        for (int i = 0; i < nums.length; i++) {
            prefix += nums[i] == 1 ? 1 : -1;

            if (map.containsKey(prefix)) {
                answer = Math.max(answer,
                    i - map.get(prefix));
            } else {
                map.put(prefix, i);
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
