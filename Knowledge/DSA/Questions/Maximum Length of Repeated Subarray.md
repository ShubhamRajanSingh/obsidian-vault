---
tags:
  - dsa
  - dynamic-programming
  - arrays
  - strings
  - leetcode
leetcode_id: 718
complexity:
  time: O(m * n)
  space: O(m * n)
status: solved
language: java
---

# Maximum Length of Repeated Subarray

## Problem Link
- https://leetcode.com/problems/maximum-length-of-repeated-subarray/

## Intuition
Similar to Longest Common Substring.
DP stores longest matching suffix lengths.

## Approach
1. Create DP table.
2. If nums1[i] == nums2[j]:
   - dp[i][j] = dp[i-1][j-1] + 1
3. Track maximum.

## Java Solution
```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;

        int[][] dp = new int[m + 1][n + 1];
        int answer = 0;

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                    answer = Math.max(answer, dp[i][j]);
                }
            }
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(m * n)
- Space Complexity: O(m * n)

## Key Takeaways
- Longest common substring DP pattern.
- Matching suffix transition.
