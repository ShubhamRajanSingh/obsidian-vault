---
tags:
  - dsa
  - greedy
  - heap
  - hard
  - leetcode
  - striver
aliases:
  - Course Schedule III
  - LeetCode 630
difficulty: Hard
topic: Greedy / Heap
pattern: Greedy with max-heap for deadline scheduling
leetcode_id: 630
date: 2026-05-19
---

# Course Schedule III

## 📌 Problem Statement

> There are `n` courses. `courses[i] = [duration, lastDay]` — the course takes `duration` days and must be finished by `lastDay`. Return the maximum number of courses you can take.

**LeetCode #630** | Difficulty: 🔴 Hard | Striver SDE Sheet ✅

---

## 🧠 Intuition

Sort by deadline. Greedily take each course. If it causes the total time to exceed the deadline, swap it with the longest course taken so far (if that course is longer — we lose fewer days).

---

## ✅ Java Solution

```java
class Solution {
    public int scheduleCourse(int[][] courses) {
        Arrays.sort(courses, (a, b) -> a[1] - b[1]);
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
        int time = 0;

        for (int[] c : courses) {
            time += c[0];
            pq.offer(c[0]);
            if (time > c[1]) time -= pq.poll(); // remove longest course
        }

        return pq.size();
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n log n) |
| Space | O(n) |
