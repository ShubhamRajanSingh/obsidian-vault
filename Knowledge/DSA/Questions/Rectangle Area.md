---
tags:
  - dsa
  - math
  - geometry
  - medium
  - leetcode
aliases:
  - Rectangle Area
  - LeetCode 223
difficulty: Medium
topic: Math / Geometry
pattern: Inclusion-exclusion principle
leetcode_id: 223
date: 2026-05-19
---

# Rectangle Area

## 📌 Problem Statement

> Given the coordinates of two axis-aligned rectangles, return the total area covered by both rectangles.

**LeetCode #223** | Difficulty: 🟡 Medium

---

## 🧠 Intuition

Total area = area1 + area2 - overlap. Overlap exists only if rectangles intersect; compute it using `max(0, min(right edges) - max(left edges))` for both dimensions.

---

## ✅ Java Solution

```java
class Solution {
    public int computeArea(int ax1, int ay1, int ax2, int ay2,
                           int bx1, int by1, int bx2, int by2) {
        int area1 = (ax2 - ax1) * (ay2 - ay1);
        int area2 = (bx2 - bx1) * (by2 - by1);

        int overlapX = Math.max(0, Math.min(ax2, bx2) - Math.max(ax1, bx1));
        int overlapY = Math.max(0, Math.min(ay2, by2) - Math.max(ay1, by1));
        int overlap = overlapX * overlapY;

        return area1 + area2 - overlap;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(1) |
| Space | O(1) |
