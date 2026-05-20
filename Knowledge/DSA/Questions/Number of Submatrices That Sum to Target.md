---
tags:
  - dsa
  - prefix-sum
  - hashmap
  - matrix
  - leetcode
leetcode_id: 1074
complexity:
  time: O(cols^2 * rows)
  space: O(rows)
status: solved
language: java
---

# Number of Submatrices That Sum to Target

## Problem Link
- https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/

## Intuition
Convert 2D problem into multiple 1D subarray sum problems using column compression.

## Approach
1. Fix left column.
2. Expand right column.
3. Build row sums.
4. Count subarrays with target sum using hashmap.

## Java Solution
```java
class Solution {
    public int numSubmatrixSumTarget(int[][] matrix, int target) {
        int rows = matrix.length;
        int cols = matrix[0].length;

        int answer = 0;

        for (int left = 0; left < cols; left++) {
            int[] sums = new int[rows];

            for (int right = left; right < cols; right++) {
                for (int r = 0; r < rows; r++) {
                    sums[r] += matrix[r][right];
                }

                answer += subarraySum(sums, target);
            }
        }

        return answer;
    }

    private int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);

        int prefix = 0;
        int count = 0;

        for (int num : nums) {
            prefix += num;

            count += map.getOrDefault(prefix - k, 0);
            map.put(prefix, map.getOrDefault(prefix, 0) + 1);
        }

        return count;
    }
}
```

## Complexity Analysis
- Time Complexity: O(cols^2 * rows)
- Space Complexity: O(rows)

## Key Takeaways
- 2D prefix compression technique.
- Reduction to subarray sum equals k.
