---
tags:
  - dsa
  - binary-search
  - design
  - arrays
  - hashmap
  - leetcode
leetcode_id: 1146
complexity:
  time: O(log n)
  space: O(total updates)
status: solved
language: java
---

# Snapshot Array

## Problem Link
- https://leetcode.com/problems/snapshot-array/

## Intuition
Store only changed values per snapshot.
Binary search retrieves historical values efficiently.

## Approach
1. For every index store:
   - list of [snapId, value]
2. set(): append new value.
3. snap(): increment snapshot id.
4. get(): binary search latest valid snapshot.

## Java Solution
```java
class SnapshotArray {

    List<int[]>[] history;
    int snapId;

    public SnapshotArray(int length) {
        history = new ArrayList[length];

        for (int i = 0; i < length; i++) {
            history[i] = new ArrayList<>();
            history[i].add(new int[]{0, 0});
        }

        snapId = 0;
    }

    public void set(int index, int val) {
        List<int[]> list = history[index];

        if (list.get(list.size() - 1)[0] == snapId) {
            list.get(list.size() - 1)[1] = val;
        } else {
            list.add(new int[]{snapId, val});
        }
    }

    public int snap() {
        return snapId++;
    }

    public int get(int index, int snap_id) {
        List<int[]> list = history[index];

        int left = 0;
        int right = list.size() - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (list.get(mid)[0] <= snap_id) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return list.get(right)[1];
    }
}
```

## Complexity Analysis
- Time Complexity:
  - set(): O(1)
  - snap(): O(1)
  - get(): O(log n)
- Space Complexity: O(total updates)

## Key Takeaways
- Persistent array concept.
- Binary search on version history.
