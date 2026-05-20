---
tags:
  - dsa
  - math
  - hashing
  - brute-force
  - leetcode
leetcode_id: 970
complexity:
  time: O(log bound * log bound)
  space: O(n)
status: solved
language: java
---

# Powerful Integers

## Problem Link
- https://leetcode.com/problems/powerful-integers/

## Intuition
Try all powers of x and y within the bound.
Use a set to avoid duplicates.

## Approach
1. Generate powers of x.
2. Generate powers of y.
3. Add sums within bound.
4. Handle x == 1 or y == 1 carefully.

## Java Solution
```java
class Solution {
    public List<Integer> powerfulIntegers(int x, int y, int bound) {
        Set<Integer> set = new HashSet<>();

        for (int a = 1; a < bound; a *= x) {
            for (int b = 1; a + b <= bound; b *= y) {
                set.add(a + b);

                if (y == 1) break;
            }

            if (x == 1) break;
        }

        return new ArrayList<>(set);
    }
}
```

## Complexity Analysis
- Time Complexity: O(log bound * log bound)
- Space Complexity: O(n)

## Key Takeaways
- Exponential growth drastically limits iterations.
- Important edge case handling.
