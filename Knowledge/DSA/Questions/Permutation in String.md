---
tags:
  - dsa
  - sliding-window
  - strings
  - hashmap
  - leetcode
leetcode_id: 567
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Permutation in String

## Problem Link
- https://leetcode.com/problems/permutation-in-string/

## Intuition
A permutation exists if some window in s2 has identical character frequencies as s1.

## Approach
1. Build frequency arrays.
2. Maintain sliding window of size s1.length().
3. Compare frequency counts.

## Java Solution
```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()) {
            return false;
        }

        int[] target = new int[26];
        int[] window = new int[26];

        for (char ch : s1.toCharArray()) {
            target[ch - 'a']++;
        }

        int size = s1.length();

        for (int i = 0; i < s2.length(); i++) {
            window[s2.charAt(i) - 'a']++;

            if (i >= size) {
                window[s2.charAt(i - size) - 'a']--;
            }

            if (Arrays.equals(target, window)) {
                return true;
            }
        }

        return false;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Fixed-size sliding window.
- Frequency matching technique.
