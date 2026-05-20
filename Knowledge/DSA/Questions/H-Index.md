---
tags:
  - dsa
  - array
  - sorting
  - medium
  - leetcode
aliases:
  - H-Index
  - LeetCode 274
difficulty: Medium
topic: Array / Sorting
pattern: Sort descending, find h where citations[h] >= h+1
leetcode_id: 274
date: 2026-05-19
---

# H-Index

## 📌 Problem Statement

> Given an array of integers `citations` (number of citations for each paper), return the researcher's h-index. The h-index is the largest value `h` such that the researcher has published at least `h` papers each cited at least `h` times.

**LeetCode #274** | Difficulty: 🟡 Medium

---

## ✅ Java Solution

```java
class Solution {
    public int hIndex(int[] citations) {
        Arrays.sort(citations);
        int n = citations.length;
        for (int i = 0; i < n; i++) {
            int h = n - i;
            if (citations[i] >= h) return h;
        }
        return 0;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log n) |
| Space | O(1) |
