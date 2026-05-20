---
tags:
  - dsa
  - greedy
  - arrays
  - simulation
  - leetcode
leetcode_id: 665
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Non-decreasing Array

## Problem Link
- https://leetcode.com/problems/non-decreasing-array/

## Intuition
At most one modification is allowed.
When violation occurs, greedily fix either current or previous element.

## Approach
1. Traverse array.
2. Count violations.
3. Adjust element strategically:
   - modify previous if safe
   - else modify current

## Java Solution
```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        int changes = 0;

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] < nums[i - 1]) {
                changes++;

                if (changes > 1) {
                    return false;
                }

                if (i == 1 || nums[i] >= nums[i - 2]) {
                    nums[i - 1] = nums[i];
                } else {
                    nums[i] = nums[i - 1];
                }
            }
        }

        return true;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Greedy local repair strategy.
- Important edge-case handling.
