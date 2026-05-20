---
tags:
  - dsa
  - arrays
  - hashmap
  - greedy
  - leetcode
leetcode_id: 2780
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Minimum Index of a Valid Split

## Problem Link
- https://leetcode.com/problems/minimum-index-of-a-valid-split/

## Intuition
We first find the dominant element using Boyer-Moore Voting Algorithm.
Then check every split position to see if the dominant element remains dominant on both sides.

## Approach
1. Find dominant candidate.
2. Count total occurrences.
3. Traverse array and maintain prefix count.
4. Validate dominance in left and right partitions.

## Java Solution
```java
class Solution {
    public int minimumIndex(List<Integer> nums) {
        int candidate = 0;
        int count = 0;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }

            count += (num == candidate) ? 1 : -1;
        }

        int total = 0;

        for (int num : nums) {
            if (num == candidate) {
                total++;
            }
        }

        int leftCount = 0;
        int n = nums.size();

        for (int i = 0; i < n - 1; i++) {
            if (nums.get(i) == candidate) {
                leftCount++;
            }

            int rightCount = total - leftCount;

            if (leftCount * 2 > (i + 1) &&
                rightCount * 2 > (n - i - 1)) {
                return i;
            }
        }

        return -1;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Boyer-Moore helps identify dominant element efficiently.
- Prefix/suffix dominance checking pattern.
