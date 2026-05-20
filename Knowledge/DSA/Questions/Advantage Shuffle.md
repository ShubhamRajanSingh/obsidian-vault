---
tags:
  - dsa
  - greedy
  - sorting
  - two-pointers
  - leetcode
leetcode_id: 870
complexity:
  time: O(n log n)
  space: O(n)
status: solved
language: java
---

# Advantage Shuffle

## Problem Link
- https://leetcode.com/problems/advantage-shuffle/

## Intuition
Greedily use the smallest number that can beat opponent's current number.
Otherwise sacrifice smallest number.

## Approach
1. Sort nums1.
2. Sort indices of nums2 by value.
3. Use two pointers:
   - assign strong values when possible
   - otherwise sacrifice weak values

## Java Solution
```java
class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        int n = nums1.length;

        Arrays.sort(nums1);

        Integer[] indices = new Integer[n];

        for (int i = 0; i < n; i++) {
            indices[i] = i;
        }

        Arrays.sort(indices,
            (a, b) -> nums2[a] - nums2[b]);

        int left = 0;
        int right = n - 1;

        int[] answer = new int[n];

        for (int num : nums1) {
            if (num > nums2[indices[left]]) {
                answer[indices[left]] = num;
                left++;
            } else {
                answer[indices[right]] = num;
                right--;
            }
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(n)

## Key Takeaways
- Greedy assignment strategy.
- Matching optimization pattern.
