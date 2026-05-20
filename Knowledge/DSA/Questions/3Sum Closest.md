---
tags:
  - dsa
  - striver-sde-sheet
  - arrays
  - two-pointers
  - leetcode
leetcode_id: 16
complexity:
  time: O(n^2)
  space: O(1)
status: solved
language: java
---

# 3Sum Closest

## Problem Link
- https://leetcode.com/problems/3sum-closest/

## Intuition
Sort the array and fix one element at a time. Then use two pointers to search for the pair that gives the closest sum to the target.

## Approach
1. Sort the array.
2. Iterate through every index `i`.
3. Use two pointers:
   - `left = i + 1`
   - `right = n - 1`
4. Compute current sum.
5. Update answer if current sum is closer.
6. Move pointers accordingly.

## Java Solution
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int closest = nums[0] + nums[1] + nums[2];

        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];

                if (Math.abs(target - sum) < Math.abs(target - closest)) {
                    closest = sum;
                }

                if (sum < target) {
                    left++;
                } else if (sum > target) {
                    right--;
                } else {
                    return sum;
                }
            }
        }

        return closest;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^2)
- Space Complexity: O(1)

## Key Takeaways
- Sorting enables efficient two-pointer traversal.
- Similar pattern to classic 3Sum.
