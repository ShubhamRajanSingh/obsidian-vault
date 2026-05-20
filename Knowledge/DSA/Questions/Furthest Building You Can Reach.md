---
tags:
  - dsa
  - heap
  - array
  - medium
  - leetcode
  - striver
aliases:
  - Furthest Building You Can Reach
  - LeetCode 1642
difficulty: Medium
topic: Greedy / Heap
pattern: Min-heap to track ladder usage
leetcode_id: 1642
date: 2026-05-19
---

# Furthest Building You Can Reach

## 📌 Problem Statement

> Given heights, `bricks`, and `ladders`, you move from building to building. For an upward jump you can use bricks (exact amount) or a ladder (any height). Return the furthest building you can reach.

**LeetCode #1642** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## 🧠 Intuition

Greedily use ladders for the largest jumps. Use a min-heap to track ladder-allocated jumps; when we run out of ladders, replace the smallest ladder jump with bricks if possible.

---

## ✅ Java Solution

```java
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(); // min-heap of ladder jumps

        for (int i = 0; i < heights.length - 1; i++) {
            int diff = heights[i + 1] - heights[i];
            if (diff <= 0) continue;

            pq.offer(diff);
            if (pq.size() > ladders) bricks -= pq.poll(); // use bricks for smallest jump
            if (bricks < 0) return i;
        }

        return heights.length - 1;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log ladders) |
| Space | O(ladders) |
