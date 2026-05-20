---
tags:
  - dsa
  - prefix-sum
  - hashmap
  - arrays
  - leetcode
leetcode_id: 560
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Subarray Sum Equals K

## Problem Link
- https://leetcode.com/problems/subarray-sum-equals-k/

## Intuition
If:
currentPrefix - previousPrefix = k
then the subarray between them sums to k.

## Approach
1. Maintain prefix sum.
2. Store frequency of prefix sums.
3. For each position:
   count += occurrences(prefix - k)
4. Update current prefix frequency.

## Java Solution
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);

        int prefix = 0;
        int count = 0;

        for (int num : nums) {
            prefix += num;

            count += map.getOrDefault(prefix - k, 0);

            map.put(prefix,
                map.getOrDefault(prefix, 0) + 1);
        }

        return count;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Prefix sum frequency mapping.
- Subarray counting optimization.
