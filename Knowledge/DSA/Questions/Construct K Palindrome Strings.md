---
tags:
  - dsa
  - greedy
  - strings
  - leetcode
leetcode_id: 1400
complexity:
  time: O(n)
  space: O(1)
status: solved
language: java
---

# Construct K Palindrome Strings

## Problem Link
- https://leetcode.com/problems/construct-k-palindrome-strings/

## Intuition
A palindrome can contain at most one odd frequency character.
Therefore:
- Minimum number of palindromes required = number of odd frequency characters.
- Maximum palindromes possible = length of string.

## Approach
1. Count character frequencies.
2. Count how many characters have odd frequency.
3. Return true if:
   - oddCount <= k
   - k <= s.length()

## Java Solution
```java
class Solution {
    public boolean canConstruct(String s, int k) {
        if (k > s.length()) return false;

        int[] freq = new int[26];

        for (char ch : s.toCharArray()) {
            freq[ch - 'a']++;
        }

        int oddCount = 0;

        for (int count : freq) {
            if (count % 2 == 1) {
                oddCount++;
            }
        }

        return oddCount <= k;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1)

## Key Takeaways
- Odd frequency characters determine minimum palindrome count.
- Greedy observation-based problem.
