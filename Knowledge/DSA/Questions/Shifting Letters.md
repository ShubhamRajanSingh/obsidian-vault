---
tags:
  - dsa
  - prefix-sum
  - strings
  - arrays
  - leetcode
leetcode_id: 848
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Shifting Letters

## Problem Link
- https://leetcode.com/problems/shifting-letters/

## Intuition
Each shift affects all previous characters.
Suffix cumulative sums help efficiently compute total shifts.

## Approach
1. Traverse shifts from right to left.
2. Maintain cumulative shift.
3. Apply modulo 26.

## Java Solution
```java
class Solution {
    public String shiftingLetters(String s, int[] shifts) {
        char[] chars = s.toCharArray();

        long total = 0;

        for (int i = shifts.length - 1; i >= 0; i--) {
            total += shifts[i];
            total %= 26;

            chars[i] = (char)(
                ((chars[i] - 'a' + total) % 26) + 'a'
            );
        }

        return new String(chars);
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Suffix accumulation technique.
- Modular arithmetic handling.
