---
tags:
  - dsa
  - array
  - binary-search
  - medium
  - leetcode
aliases:
  - Find the Duplicate Number
  - LeetCode 287
difficulty: Medium
topic: Array / Binary Search / Floyd's Cycle
pattern: Floyd's cycle detection or binary search
leetcode_id: 287
date: 2026-05-19
---

# Find the Duplicate Number

## 📌 Problem Statement

> Given an array of integers `nums` containing n+1 integers in range [1, n], prove that at least one duplicate must exist. Find it without modifying the array and using O(1) extra space.

**LeetCode #287** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution (Floyd's Cycle Detection)

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0], fast = nums[0];
        do { slow = nums[slow]; fast = nums[nums[fast]]; } while (slow != fast);
        slow = nums[0];
        while (slow != fast) { slow = nums[slow]; fast = nums[fast]; }
        return slow;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O(n) |
| Space | O(1) |
