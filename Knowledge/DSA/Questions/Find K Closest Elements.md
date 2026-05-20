---
tags:
  - dsa
  - binary-search
  - two-pointers
  - arrays
  - leetcode
leetcode_id: 658
complexity:
  time: O(log(n-k))
  space: O(1)
status: solved
language: java
---

# Find K Closest Elements

## Problem Link
- https://leetcode.com/problems/find-k-closest-elements/

## Intuition
Binary search can identify the best starting window of size k.

## Approach
1. Binary search on window start.
2. Compare distances:
   - x - arr[mid]
   - arr[mid + k] - x
3. Return resulting window.

## Java Solution
```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int left = 0;
        int right = arr.length - k;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (x - arr[mid] > arr[mid + k] - x) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        List<Integer> result = new ArrayList<>();

        for (int i = left; i < left + k; i++) {
            result.add(arr[i]);
        }

        return result;
    }
}
```

## Complexity Analysis
- Time Complexity: O(log(n-k) + k)
- Space Complexity: O(1)

## Key Takeaways
- Binary search on answer/window.
- Sliding window optimization.
