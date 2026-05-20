---
tags:
  - dsa
  - intervals
  - treeset
  - design
  - leetcode
leetcode_id: 729
complexity:
  time: O(log n)
  space: O(n)
status: solved
language: java
---

# My Calendar I

## Problem Link
- https://leetcode.com/problems/my-calendar-i/

## Intuition
Intervals cannot overlap.
TreeMap helps locate neighboring intervals efficiently.

## Approach
1. Find previous booking.
2. Find next booking.
3. Check overlap conditions.
4. Insert if safe.

## Java Solution
```java
class MyCalendar {

    TreeMap<Integer, Integer> calendar;

    public MyCalendar() {
        calendar = new TreeMap<>();
    }

    public boolean book(int start, int end) {
        Integer prev = calendar.floorKey(start);
        Integer next = calendar.ceilingKey(start);

        if ((prev == null || calendar.get(prev) <= start) &&
            (next == null || end <= next)) {

            calendar.put(start, end);
            return true;
        }

        return false;
    }
}
```

## Complexity Analysis
- Time Complexity: O(log n)
- Space Complexity: O(n)

## Key Takeaways
- Ordered interval management.
- TreeMap neighbor lookup pattern.
