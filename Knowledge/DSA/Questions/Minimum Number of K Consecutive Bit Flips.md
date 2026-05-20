---
tags:
  - dsa
  - greedy
  - sliding-window
  - arrays
  - leetcode
leetcode_id: 995
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Minimum Number of K Consecutive Bit Flips

## Problem Link
- https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/

## Intuition
Track whether current bit has been flipped odd or even number of times.
Greedily flip whenever current effective bit becomes 0.

## Approach
1. Use queue/array to track flip expiration.
2. Maintain active flip parity.
3. If effective bit is 0, flip window.
4. If window exceeds bounds, return -1.

## Java Solution
```java
class Solution {
    public int minKBitFlips(int[] nums, int k) {
        int n = nums.length;
        int[] flipped = new int[n];

        int flip = 0;
        int answer = 0;

        for (int i = 0; i < n; i++) {
            if (i >= k) {
                flip ^= flipped[i - k];
            }

            if (nums[i] == flip) {
                if (i + k > n) {
                    return -1;
                }

                flipped[i] = 1;
                flip ^= 1;
                answer++;
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
- Lazy flip propagation technique.
- Greedy sliding window optimization.
