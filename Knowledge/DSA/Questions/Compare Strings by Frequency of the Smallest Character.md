---
tags:
  - dsa
  - binary-search
  - strings
  - sorting
  - leetcode
leetcode_id: 1170
complexity:
  time: O(n log n)
  space: O(n)
status: solved
language: java
---

# Compare Strings by Frequency of the Smallest Character

## Problem Link
- https://leetcode.com/problems/compare-strings-by-frequency-of-the-smallest-character/

## Intuition
Compute smallest-character frequency for each word.
Sort word frequencies and binary search answers.

## Approach
1. Compute frequencies for words.
2. Sort frequencies.
3. For every query:
   - compute frequency
   - binary search greater counts.

## Java Solution
```java
class Solution {
    public int[] numSmallerByFrequency(String[] queries,
                                       String[] words) {

        int[] freq = new int[words.length];

        for (int i = 0; i < words.length; i++) {
            freq[i] = smallestFreq(words[i]);
        }

        Arrays.sort(freq);

        int[] answer = new int[queries.length];

        for (int i = 0; i < queries.length; i++) {
            int value = smallestFreq(queries[i]);

            int idx = upperBound(freq, value);
            answer[i] = freq.length - idx;
        }

        return answer;
    }

    private int smallestFreq(String s) {
        char smallest = 'z';
        int count = 0;

        for (char ch : s.toCharArray()) {
            if (ch < smallest) {
                smallest = ch;
                count = 1;
            } else if (ch == smallest) {
                count++;
            }
        }

        return count;
    }

    private int upperBound(int[] arr, int target) {
        int left = 0;
        int right = arr.length;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (arr[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        return left;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(n)

## Key Takeaways
- Frequency transformation.
- Binary search counting.
