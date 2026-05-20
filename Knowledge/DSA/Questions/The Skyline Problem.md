---
tags:
  - dsa
  - heap
  - divide-and-conquer
  - hard
  - leetcode
  - striver
aliases:
  - The Skyline Problem
  - LeetCode 218
difficulty: Hard
topic: Heap / Divide and Conquer
pattern: Max-heap of active buildings
leetcode_id: 218
date: 2026-05-19
---

# The Skyline Problem

## 📌 Problem Statement

> Given a list of buildings `[left, right, height]`, return the skyline formed by these buildings as a list of key points.

**LeetCode #218** | Difficulty: 🔴 Hard | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public List<List<Integer>> getSkyline(int[][] buildings) {
        List<int[]> events = new ArrayList<>();
        for (int[] b : buildings) {
            events.add(new int[]{b[0], -b[2]}); // start: negative height
            events.add(new int[]{b[1], b[2]});  // end: positive height
        }
        events.sort((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);

        TreeMap<Integer, Integer> maxHeap = new TreeMap<>(Collections.reverseOrder());
        maxHeap.put(0, 1);
        int prevMax = 0;
        List<List<Integer>> res = new ArrayList<>();

        for (int[] e : events) {
            if (e[1] < 0) maxHeap.merge(-e[1], 1, Integer::sum);
            else {
                int cnt = maxHeap.get(e[1]);
                if (cnt == 1) maxHeap.remove(e[1]);
                else maxHeap.put(e[1], cnt - 1);
            }
            int currMax = maxHeap.firstKey();
            if (currMax != prevMax) {
                res.add(Arrays.asList(e[0], currMax));
                prevMax = currMax;
            }
        }
        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log n) |
| Space | O(n) |
