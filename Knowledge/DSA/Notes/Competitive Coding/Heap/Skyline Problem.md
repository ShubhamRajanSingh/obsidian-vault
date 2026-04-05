# 🏙️ Skyline Problem

#tags: #dsa #heap #sweepline #hard

## 📌 Problem
Given buildings, return skyline key points.

---

## 🧠 Approach: Sweep Line + Max Heap

### Steps:
1. Create events (start + end)
2. Sort by x-coordinate
3. Use max heap to track heights
4. Add key points when height changes

---

## ⚡ Key Idea
- Convert buildings → events
- Maintain active buildings

---

## 🧠 Complexity
- O(n log n)

---

## ⚠️ Notes
- Very important interview problem
- Tests heap + sorting + logic
