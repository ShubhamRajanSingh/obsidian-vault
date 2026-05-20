---
tags:
  - dsa
  - prefix-sum
  - arrays
  - hashing
  - leetcode
leetcode_id: 548
complexity:
  time: O(n^2)
  space: O(n)
status: solved
language: java
---

# Split Array with Equal Sum

## Problem Link
- https://leetcode.com/problems/split-array-with-equal-sum/

## Intuition
Prefix sums allow quick subarray sum calculation.
We try all middle split positions and validate equal partitions.

## Approach
1. Build prefix sum array.
2. Iterate middle split j.
3. Store valid left partition sums.
4. Validate right-side partitions.

## Java Solution
```java
class Solution {
    public boolean splitArray(int[] nums) {
        int n = nums.length;

        int[] prefix = new int[n];
        prefix[0] = nums[0];

        for (int i = 1; i < n; i++) {
            prefix[i] = prefix[i - 1] + nums[i];
        }

        for (int j = 3; j < n - 3; j++) {
            Set<Integer> set = new HashSet<>();

            for (int i = 1; i < j - 1; i++) {
                int left = prefix[i - 1];
                int middle = prefix[j - 1] - prefix[i];

                if (left == middle) {
                    set.add(left);
                }
            }

            for (int k = j + 2; k < n - 1; k++) {
                int middle = prefix[k - 1] - prefix[j];
                int right = prefix[n - 1] - prefix[k];

                if (middle == right &&
                    set.contains(middle)) {

                    return true;
                }
            }
        }

        return false;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^2)
- Space Complexity: O(n)

## Key Takeaways
- Prefix sum partitioning.
- Multi-part equality checking.
