---
tags:
  - dsa
  - greedy
  - strings
  - heap
  - leetcode
leetcode_id: 984
complexity:
  time: O(a + b)
  space: O(1)
status: solved
language: java
---

# String Without AAA or BBB

## Problem Link
- https://leetcode.com/problems/string-without-aaa-or-bbb/

## Intuition
Always place the character with larger remaining count unless it creates three consecutive characters.

## Approach
1. Greedily append dominant character.
2. Prevent three consecutive identical characters.
3. Alternate when needed.

## Java Solution
```java
class Solution {
    public String strWithout3a3b(int a, int b) {
        StringBuilder sb = new StringBuilder();

        while (a > 0 || b > 0) {
            int len = sb.length();

            boolean writeA = false;

            if (len >= 2 &&
                sb.charAt(len - 1) == sb.charAt(len - 2)) {

                writeA = sb.charAt(len - 1) == 'b';
            } else {
                writeA = a >= b;
            }

            if (writeA) {
                sb.append('a');
                a--;
            } else {
                sb.append('b');
                b--;
            }
        }

        return sb.toString();
    }
}
```

## Complexity Analysis
- Time Complexity: O(a + b)
- Space Complexity: O(1)

## Key Takeaways
- Greedy character placement.
- Constraint-aware construction problems.
