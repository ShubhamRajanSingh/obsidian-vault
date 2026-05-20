---
tags:
  - dsa
  - intervals
  - design
  - treeset
  - leetcode
leetcode_id: 2276
complexity:
  time: O(log n)
  space: O(n)
status: solved
language: java
---

# Count Integers in Intervals

## Problem Link
- https://leetcode.com/problems/count-integers-in-intervals/

## Intuition
Merge overlapping intervals dynamically while maintaining total covered length.

## Approach
1. Store intervals in TreeMap.
2. Merge overlapping ranges during insertion.
3. Maintain running count of covered integers.

## Java Solution
```java
class CountIntervals {
    TreeMap<Integer, Integer> map;
    int total;

    public CountIntervals() {
        map = new TreeMap<>();
        total = 0;
    }

    public void add(int left, int right) {
        Integer start = map.floorKey(right);

        while (start != null && map.get(start) >= left - 1) {
            int end = map.get(start);

            left = Math.min(left, start);
            right = Math.max(right, end);

            total -= (end - start + 1);
            map.remove(start);

            start = map.floorKey(right);
        }

        map.put(left, right);
        total += (right - left + 1);
    }

    public int count() {
        return total;
    }
}
```

## Complexity Analysis
- Time Complexity: O(log n) amortized
- Space Complexity: O(n)

## Key Takeaways
- Interval merging pattern.
- TreeMap useful for ordered interval queries.
