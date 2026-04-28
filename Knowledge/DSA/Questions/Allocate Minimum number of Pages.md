---

tags:

- dsa
- binary-search
- array
- medium
- greedy
- gfg
- classic aliases:
- Allocate Minimum Number of Pages
- Book Allocation Problem difficulty: Medium topic: Binary Search on Answer pattern: Binary Search + Feasibility Check date: 2026-04-25 related: "[[Kth Element of Two Sorted Arrays]]"

---
#REVISIT 

# Allocate Minimum Number of Pages

## 📌 Problem Statement

> Given an array `arr[]` where `arr[i]` is the number of pages in the `i-th` book, and `m` students, allocate books to students such that:
> 
> - Each student gets **at least one book**
> - Each book is allocated to **exactly one student**
> - Books are allocated in **contiguous order** (a student gets a contiguous subarray of books)
> - The **maximum pages** assigned to any student is **minimized**

Return the **minimum possible value** of the maximum pages any student has to read.  
Return `-1` if allocation is impossible.

### Example 1

```
arr   = [12, 34, 67, 90]
m     = 2

Possible allocations:
  Student 1: [12]        Student 2: [34, 67, 90]  → max = 191
  Student 1: [12, 34]    Student 2: [67, 90]      → max = 157  ✅ minimum
  Student 1: [12, 34, 67] Student 2: [90]         → max = 113

Wait — 113 is less than 157. Let's re-check:
  [12,34,67] → sum = 113
  [90]       → sum = 90
  max = 113

Ans = 113
```

### Example 2

```
arr = [15, 17, 20]
m   = 5

Students > Books → impossible → return -1
```

---

## 🧠 Intuition — Binary Search on Answer

Instead of searching **where to partition**, we binary search on the **answer itself** — the maximum pages allowed per student.

### Key Observations

|Observation|Implication|
|---|---|
|More pages allowed per student → fewer students needed|Answer space is **monotonic**|
|Fewer pages allowed → more students needed|Classic BS condition|
|Books must be contiguous|Greedy check is O(n)|

So we ask: _"If the max pages per student is `mid`, can we allocate all books to `m` students?"_

---

## 📐 Pictorial Concept

### The Answer Space

```
lo = max(arr)          hi = sum(arr)
(min possible answer)  (max possible answer — one student reads all)

   lo ←————————————————————————→ hi
   |XXXXXXXXXX|✅✅✅✅✅✅✅✅✅✅|
              ↑
         first valid mid = answer
```

- For any `mid < lo` → impossible (a single book already exceeds the limit)
- For any `mid ≥ first valid point` → possible
- We want the **leftmost valid mid**

### Greedy Check: Can we allocate with max = `mid`?

```
arr = [12, 34, 67, 90],  mid = 113

student = 1, pages = 0

book 12  → pages = 12        (12 ≤ 113 ✅)
book 34  → pages = 46        (46 ≤ 113 ✅)
book 67  → pages = 113       (113 ≤ 113 ✅)
book 90  → pages = 113+90 = 203 > 113 ❌
             → new student! student = 2, pages = 90

End: students used = 2 ≤ m=2  ✅ → mid=113 is valid
```

### What if mid is too small?

```
arr = [12, 34, 67, 90],  mid = 50

student = 1, pages = 0

book 12  → pages = 12  ✅
book 34  → pages = 46  ✅
book 67  → 46+67 = 113 > 50 ❌ → student = 2, pages = 67
             67 ≤ 50? NO ❌ → student = 3, pages = 67... wait
             67 > 50 alone! → invalid immediately

Students needed > m → mid=50 is NOT valid → search right
```

---

## 🔀 Binary Search Logic

```
lo = max(arr)      ← a single book must fit within the limit
hi = sum(arr)      ← worst case: one student gets everything

while lo <= hi:
    mid = (lo + hi) / 2

    if canAllocate(arr, m, mid):
        ans = mid          ← valid, but try smaller
        hi = mid - 1
    else:
        lo = mid + 1       ← too small, increase limit
```

---

## ✅ Feasibility Check

```java
boolean canAllocate(int[] arr, int m, int maxPages) {
    int students = 1;
    int pages = 0;

    for (int book : arr) {
        if (book > maxPages) return false;  // single book exceeds limit

        if (pages + book > maxPages) {
            students++;          // assign current book to next student
            pages = book;
            if (students > m) return false;
        } else {
            pages += book;
        }
    }
    return true;
}
```

---

## ✅ Full Java Solution

```java
class Solution {
    public static int findPages(int[] arr, int m) {
        int n = arr.length;

        // Impossible: more students than books
        if (m > n) return -1;

        int lo = 0, hi = 0;
        for (int pages : arr) {
            lo = Math.max(lo, pages);  // max single book (lower bound)
            hi += pages;               // sum of all books (upper bound)
        }

        int ans = -1;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;

            if (canAllocate(arr, m, mid)) {
                ans = mid;       // valid — try to minimize further
                hi = mid - 1;
            } else {
                lo = mid + 1;    // not valid — need more pages per student
            }
        }

        return ans;
    }

    private static boolean canAllocate(int[] arr, int m, int maxPages) {
        int students = 1;
        int currentPages = 0;

        for (int book : arr) {
            if (book > maxPages) return false; // single book won't fit

            if (currentPages + book > maxPages) {
                students++;
                currentPages = book;
                if (students > m) return false;
            } else {
                currentPages += book;
            }
        }
        return true;
    }
}
```

---

## 📊 Step-by-Step Trace

### Input: `arr = [12, 34, 67, 90]`, `m = 2`

```
lo = max(12,34,67,90) = 90
hi = 12+34+67+90     = 203
```

|Iteration|lo|hi|mid|canAllocate?|Action|
|---|---|---|---|---|---|
|1|90|203|146|✅ (1 student)|ans=146, hi=145|
|2|90|145|117|✅ (2 students)|ans=117, hi=116|
|3|90|116|103|❌ (3 students)|lo=104|
|4|104|116|110|❌ (3 students)|lo=111|
|5|111|116|113|✅ (2 students)|ans=113, hi=112|
|6|111|112|111|❌ (3 students)|lo=112|
|7|112|112|112|❌ (3 students)|lo=113|

`lo > hi` → stop. **Answer = 113** ✅

### Verify canAllocate(arr, 2, 113):

```
student=1, pages=0
  +12 → 12  ≤ 113 ✅
  +34 → 46  ≤ 113 ✅
  +67 → 113 ≤ 113 ✅
  +90 → 203 > 113 → new student, pages=90

student=2, pages=90
  End of array.

students used = 2 ≤ m=2 ✅ → valid
```

---

## 🔑 Key Formulas / Bounds

```
lo  = max(arr)           ← minimum possible answer
hi  = sum(arr)           ← maximum possible answer
mid = lo + (hi - lo) / 2 ← avoids integer overflow

Feasibility:
  For each book, greedily assign to current student.
  If adding a book exceeds maxPages → start a new student.
  If students > m → infeasible.
```

---

## 🪤 Common Mistakes

|Mistake|Fix|
|---|---|
|`lo = 0` instead of `lo = max(arr)`|A student must be able to hold at least the largest single book|
|Not returning -1 when `m > n`|Can't give each student ≥ 1 book if students > books|
|Using `mid = (lo + hi) / 2` on large sums|Use `lo + (hi - lo) / 2` to avoid overflow|
|Not checking `book > maxPages` in feasibility|A single oversized book makes allocation impossible|
|Returning `minRight` instead of tracking `ans`|Always save last valid `mid` before shrinking|

---

## 🔗 Similar Problems (Same Pattern — Binary Search on Answer)

|Problem|What you're searching|Feasibility check|
|---|---|---|
|**Allocate Pages**|Min of max pages|Greedy partition ≤ m students|
|Painter's Partition|Min of max work|Greedy partition ≤ k painters|
|Koko Eating Bananas|Min eating speed|Hours needed ≤ h|
|Capacity to Ship Packages|Min ship weight|Days needed ≤ d|
|Split Array Largest Sum|Min of max subarray sum|Greedy split ≤ k parts|

> All these problems share the same skeleton: **binary search on the answer + greedy feasibility check**.

---

## ⏱️ Complexity Analysis

|||
|---|---|
|**Time**|O(n log(sum)) — log(sum) binary search steps, O(n) feasibility each|
|**Space**|O(1) — no extra space|

---

## 🔗 Related Notes

- [[Kth Element of Two Sorted Arrays]]
- [[Median of Two Sorted Arrays]]
- [[Binary Search]]