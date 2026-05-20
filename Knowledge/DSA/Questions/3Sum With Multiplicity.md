---
tags:
  - dsa
  - two-pointers
  - arrays
  - combinatorics
  - leetcode
leetcode_id: 923
complexity:
  time: O(n^2)
  space: O(1)
status: solved
language: java
---

# 3Sum With Multiplicity

## Problem Link
- https://leetcode.com/problems/3sum-with-multiplicity/

## Intuition
Sort the array and count valid triplets carefully while handling duplicates.
Use combinatorics for repeated values.

## Approach
1. Sort array.
2. Fix first element.
3. Use two pointers.
4. Count duplicate combinations efficiently.

## Java Solution
```java
class Solution {
    public int threeSumMulti(int[] arr, int target) {
        Arrays.sort(arr);

        long mod = 1_000_000_007;
        long answer = 0;

        for (int i = 0; i < arr.length; i++) {
            int left = i + 1;
            int right = arr.length - 1;

            while (left < right) {
                int sum = arr[i] + arr[left] + arr[right];

                if (sum < target) {
                    left++;
                } else if (sum > target) {
                    right--;
                } else {
                    if (arr[left] == arr[right]) {
                        long count = right - left + 1;
                        answer += count * (count - 1) / 2;
                        answer %= mod;
                        break;
                    }

                    int leftCount = 1;
                    int rightCount = 1;

                    while (left + 1 < right &&
                           arr[left] == arr[left + 1]) {
                        leftCount++;
                        left++;
                    }

                    while (right - 1 > left &&
                           arr[right] == arr[right - 1]) {
                        rightCount++;
                        right--;
                    }

                    answer += (long) leftCount * rightCount;
                    answer %= mod;

                    left++;
                    right--;
                }
            }
        }

        return (int) answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n^2)
- Space Complexity: O(1)

## Key Takeaways
- Duplicate handling in two-pointer problems.
- Combinatorics counting optimization.
