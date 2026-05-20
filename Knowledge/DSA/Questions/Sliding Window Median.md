---
tags:
  - dsa
  - heap
  - sliding-window
  - hard
  - leetcode
aliases:
  - Sliding Window Median
  - LeetCode 480
difficulty: Hard
topic: Heap / Sliding Window
pattern: Two heaps (max-heap + min-heap) with lazy deletion
leetcode_id: 480
date: 2026-05-19
---

# Sliding Window Median

## 📌 Problem Statement

> Given an integer array `nums` and a sliding window of size `k`, return the median array for each window position.

**LeetCode #480** | Difficulty: 🔴 Hard

---

## 🧠 Intuition

Maintain a max-heap (lower half) and min-heap (upper half). When the window slides, add new element and remove outgoing element using lazy deletion. Rebalance heaps to maintain size property.

---

## ✅ Java Solution

```java
class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        TreeMap<Integer, Integer> lo = new TreeMap<>(Collections.reverseOrder());
        TreeMap<Integer, Integer> hi = new TreeMap<>();
        double[] res = new double[nums.length - k + 1];

        for (int i = 0; i < nums.length; i++) {
            add(lo, hi, nums[i]);
            if (i >= k) remove(lo, hi, nums[i - k]);
            if (i >= k - 1) {
                if (k % 2 == 0) res[i - k + 1] = ((double) lo.firstKey() + hi.firstKey()) / 2.0;
                else res[i - k + 1] = k % 2 == 1 ? (lo.size() > hi.size() ? lo.firstKey() : hi.firstKey()) : ((double) lo.firstKey() + hi.firstKey()) / 2.0;
            }
        }
        return res;
    }

    int loSize = 0, hiSize = 0;

    void add(TreeMap<Integer, Integer> lo, TreeMap<Integer, Integer> hi, int num) {
        lo.merge(num, 1, Integer::sum); loSize++;
        hi.merge(lo.firstKey(), 1, Integer::sum);
        lo.merge(lo.firstKey(), -1, Integer::sum);
        lo.entrySet().removeIf(e -> e.getValue() == 0);
        loSize--; hiSize++;
        if (hiSize > loSize) {
            lo.merge(hi.firstKey(), 1, Integer::sum); loSize++;
            hi.merge(hi.firstKey(), -1, Integer::sum);
            hi.entrySet().removeIf(e -> e.getValue() == 0);
            hiSize--;
        }
    }

    void remove(TreeMap<Integer, Integer> lo, TreeMap<Integer, Integer> hi, int num) {
        if (lo.containsKey(num)) { lo.merge(num, -1, Integer::sum); lo.entrySet().removeIf(e -> e.getValue() == 0); loSize--; }
        else { hi.merge(num, -1, Integer::sum); hi.entrySet().removeIf(e -> e.getValue() == 0); hiSize--; }
        if (loSize < hiSize) {
            lo.merge(hi.firstKey(), 1, Integer::sum); loSize++;
            hi.merge(hi.firstKey(), -1, Integer::sum);
            hi.entrySet().removeIf(e -> e.getValue() == 0); hiSize--;
        } else if (loSize > hiSize + 1) {
            hi.merge(lo.firstKey(), 1, Integer::sum); hiSize++;
            lo.merge(lo.firstKey(), -1, Integer::sum);
            lo.entrySet().removeIf(e -> e.getValue() == 0); loSize--;
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log k) |
| Space | O(k) |
