---
tags:
  - dsa
  - greedy
  - strings
  - stack
  - leetcode
leetcode_id: 678
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Valid Parenthesis String

## Problem Link
- https://leetcode.com/problems/valid-parenthesis-string/

## Intuition
'*' can act as:
- '('
- ')'
- empty

Track range of possible open brackets greedily.

## Approach
1. Maintain:
   - low = minimum open count
   - high = maximum open count
2. Update counts per character.
3. Invalid if high becomes negative.
4. Valid if low ends at 0.

## Java Solution
```java
class Solution {
    public boolean checkValidString(String s) {
        int low = 0;
        int high = 0;

        for (char ch : s.toCharArray()) {
            if (ch == '(') {
                low++;
                high++;
            } else if (ch == ')') {
                low--;
                high--;
            } else {
                low--;
                high++;
            }

            if (high < 0) {
                return false;
            }

            low = Math.max(low, 0);
        }

        return low == 0;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Greedy range tracking.
- Flexible state compression.
