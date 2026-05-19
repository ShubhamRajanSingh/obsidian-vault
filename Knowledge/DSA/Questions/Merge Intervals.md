---
tags:
  - dsa
  - array
  - sorting
  - medium
  - leetcode
  - striver
aliases:
  - Merge Intervals
  - LeetCode 56
difficulty: Medium
topic: Array / Sorting
pattern: Sort + merge overlapping
leetcode_id: 56
date: 2026-05-19
---

# Merge Intervals

## 📌 Problem Statement

> Given an array of intervals, merge all overlapping intervals and return an array of the non-overlapping intervals.

**LeetCode #56** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        List<int[]> res = new ArrayList<>();

        for (int[] interval : intervals) {
            if (res.isEmpty() || res.get(res.size() - 1)[1] < interval[0]) {
                res.add(interval);
            } else {
                res.get(res.size() - 1)[1] = Math.max(res.get(res.size() - 1)[1], interval[1]);
            }
        }

        return res.toArray(new int[0][]);
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log n) |
| Space | O(n) |
