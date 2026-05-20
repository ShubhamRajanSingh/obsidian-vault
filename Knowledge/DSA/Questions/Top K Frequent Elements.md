---
tags:
  - dsa
  - array
  - heap
  - bucket-sort
  - medium
  - leetcode
  - striver
aliases:
  - Top K Frequent Elements
  - LeetCode 347
difficulty: Medium
topic: Array / Heap / Bucket Sort
pattern: HashMap + bucket sort or min-heap
leetcode_id: 347
date: 2026-05-19
---

# Top K Frequent Elements

## 📌 Problem Statement

> Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

**LeetCode #347** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (Bucket Sort — O(n))

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();
        for (int n : nums) freq.merge(n, 1, Integer::sum);

        List<Integer>[] bucket = new List[nums.length + 1];
        for (int num : freq.keySet()) {
            int f = freq.get(num);
            if (bucket[f] == null) bucket[f] = new ArrayList<>();
            bucket[f].add(num);
        }

        List<Integer> res = new ArrayList<>();
        for (int i = bucket.length - 1; i >= 0 && res.size() < k; i--)
            if (bucket[i] != null) res.addAll(bucket[i]);

        return res.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(n) |
