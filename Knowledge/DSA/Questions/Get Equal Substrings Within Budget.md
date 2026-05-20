---
tags:
  - dsa
  - sliding-window
  - strings
  - two-pointers
  - leetcode
leetcode_id: 1208
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Get Equal Substrings Within Budget

## Problem Link
- https://leetcode.com/problems/get-equal-substrings-within-budget/

## Intuition
Use sliding window while maintaining total conversion cost.
Shrink window whenever budget exceeds maxCost.

## Approach
1. Expand right pointer.
2. Add conversion cost.
3. Shrink left while cost exceeds limit.
4. Track maximum valid window.

## Java Solution
```java
class Solution {
    public int equalSubstring(String s, String t, int maxCost) {
        int left = 0;
        int cost = 0;
        int answer = 0;

        for (int right = 0; right < s.length(); right++) {
            cost += Math.abs(s.charAt(right) - t.charAt(right));

            while (cost > maxCost) {
                cost -= Math.abs(s.charAt(left) - t.charAt(left));
                left++;
            }

            answer = Math.max(answer, right - left + 1);
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Standard variable-size sliding window.
- Budget-based window management.
