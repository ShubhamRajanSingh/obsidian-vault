---

tags:

- dsa
- binary-search
- array
- medium
- divide-and-conquer
- two-pointers
- gfg aliases:
- Kth Element of Two Sorted Arrays
- Kth Smallest in Two Sorted Arrays difficulty: Medium topic: Binary Search pattern: Partition Array date: 2026-04-25 related: "[[Median of Two Sorted Arrays]]"

---

# Kth Element of Two Sorted Arrays

## 📌 Problem Statement

> Given two sorted arrays `arr1` of size `m` and `arr2` of size `n`, and an integer `k`, find the **k-th smallest element** in the combined sorted order of both arrays.

### Example 1

```
arr1 = [2, 3, 6, 7, 9]
arr2 = [1, 4, 8, 10]
k    = 5

Merged (sorted) = [1, 2, 3, 4, 6, 7, 8, 9, 10]
                         ↑
                       k=5 → Answer = 6
```

### Example 2

```
arr1 = [100, 112, 256, 349, 770]
arr2 = [72, 86, 113, 119, 265, 445, 892]
k    = 7

Merged (sorted) = [72, 86, 100, 112, 113, 119, 256, ...]
                                              ↑
                                           k=7 → Answer = 256
```

---

## 🧠 Intuition

The brute-force approach is to merge both arrays and return the k-th element — but that's **O(m + n)** time and space.

The smarter approach: we don't need to merge. Instead, we use **binary search on a partition**, similar to [[Median of Two Sorted Arrays]], but now we're looking for the **k-th position** rather than the middle.

### Core Idea

Place a **cut** across both arrays so that exactly **k elements** lie to the left of the cut combined:

```
arr1: [ ← cut1 elements → | ... ]
arr2: [ ← cut2 elements → | ... ]

where  cut1 + cut2 = k
```

The k-th smallest element is:

```
max(arr1[cut1 - 1], arr2[cut2 - 1])
```

...when the partition is valid (left side ≤ right side across both arrays).

---

## ✂️ The Partition Idea

### What makes a partition valid?

```
arr1:  [ ... left1 | right1 ... ]
arr2:  [ ... left2 | right2 ... ]
```

|Condition|Meaning|
|---|---|
|`cut1 + cut2 == k`|Exactly k elements on the left|
|`arr1[cut1-1] ≤ arr2[cut2]`|Left of arr1 ≤ Right of arr2|
|`arr2[cut2-1] ≤ arr1[cut1]`|Left of arr2 ≤ Right of arr1|

When both cross-conditions hold, the k-th element is `max(arr1[cut1-1], arr2[cut2-1])`.

---

## 📐 Pictorial Walkthrough

### Setup

```
arr1 = [2, 3, 6, 7, 9]     m = 5
arr2 = [1, 4, 8, 10]       n = 4
k = 5
```

Binary search `cut1` over `arr1` in range `[lo, hi]`:

```
lo = max(0, k - n) = max(0, 5 - 4) = 1
hi = min(k, m)     = min(5, 5)     = 5
```

---

#### Iteration 1: cut1 = 3, cut2 = k - cut1 = 2

```
            cut1 = 3
                ↓
arr1:  [ 2  3  6 | 7  9 ]
        ←left1→   ←right1→

        cut2 = 2
            ↓
arr2:  [ 1  4 | 8  10 ]
        ←left2→  ←right2→

maxLeft1  = arr1[2] = 6
minRight1 = arr1[3] = 7
maxLeft2  = arr2[1] = 4
minRight2 = arr2[2] = 8

Check: maxLeft1(6) ≤ minRight2(8) ✅
Check: maxLeft2(4) ≤ minRight1(7) ✅

✅ Valid Partition!
k-th element = max(maxLeft1, maxLeft2) = max(6, 4) = 6
```

---

#### What if cut1 is too small?

```
            cut1 = 1
            ↓
arr1:  [ 2 | 3  6  7  9 ]

                cut2 = 4
                        ↓
arr2:  [ 1  4  8  10 | ]

maxLeft1  = arr1[0] = 2
minRight1 = arr1[1] = 3
maxLeft2  = arr2[3] = 10
minRight2 = +∞

maxLeft2(10) > minRight1(3)  ❌
→ cut1 is too small → move RIGHT (lo = cut1 + 1)
```

---

#### What if cut1 is too large?

```
                    cut1 = 5
                            ↓
arr1:  [ 2  3  6  7  9 | ]

cut2 = 0
↓
arr2:  [ | 1  4  8  10 ]

maxLeft1  = arr1[4] = 9
minRight1 = +∞
maxLeft2  = -∞
minRight2 = arr2[0] = 1

maxLeft1(9) > minRight2(1)  ❌
→ cut1 is too large → move LEFT (hi = cut1 - 1)
```

---

## 🔀 Binary Search Decision Logic

```
lo = max(0, k - n)
hi = min(k, m)

while lo <= hi:
    cut1 = (lo + hi) / 2
    cut2 = k - cut1

    maxLeft1  = (cut1 == 0) ? -∞ : arr1[cut1 - 1]
    minRight1 = (cut1 == m) ? +∞ : arr1[cut1]
    maxLeft2  = (cut2 == 0) ? -∞ : arr2[cut2 - 1]
    minRight2 = (cut2 == n) ? +∞ : arr2[cut2]

    if maxLeft1 > minRight2 → cut1 too large → hi = cut1 - 1
    if maxLeft2 > minRight1 → cut1 too small → lo = cut1 + 1
    else → FOUND → return max(maxLeft1, maxLeft2)
```

---

## 🔢 Why `lo = max(0, k-n)` and `hi = min(k, m)`?

Since `cut2 = k - cut1`, the value of `cut1` can't be just any number — it has **two constraints pulling on it from both sides**.

---

### `hi = min(k, m)` — Upper Limit of cut1

`cut1` has an upper limit from **two sources**:

**Source 1 — Can't exceed array size:**

```
cut1 ≤ m
(you can't take more elements than arr1 has)
```

**Source 2 — Can't take more than k total:**

```
cut1 ≤ k
(we only need k elements on the left in total,
 so cut1 alone can never exceed k)
```

We take the stricter (smaller) of the two:

```
hi = min(k, m)
```

**Example:** `m = 10, n = 4, k = 5`

```
min(5, 10) = 5
→ cut1 can go at most up to 5
  (limited by k here, not by array size)
```

---

### `lo = max(0, k-n)` — Lower Limit of cut1

`cut1` has a lower limit from **two sources**:

**Source 1 — Can't be negative:**

```
cut1 ≥ 0
(obvious — can't take negative elements)
```

**Source 2 — cut2 can't exceed arr2's size:**

```
cut2 = k - cut1 ≤ n
→ k - cut1 ≤ n
→ cut1 ≥ k - n
```

We take the stricter (larger) of the two:

```
lo = max(0, k - n)
```

**Example:** `m = 5, n = 4, k = 5`

```
k - n = 5 - 4 = 1
max(0, 1) = 1
→ cut1 must be at least 1

Why? If cut1 = 0 → cut2 = 5
But arr2 only has 4 elements → cut2 = 5 is OUT OF BOUNDS ❌
```

---

### Visual Summary

```
              cut1 can be...

  ←————————————————————————————————————→
   0    1    2    3    4    5    6 ...

   |←— INVALID ——→|←—— VALID ——→|←— INVALID ——→|
          ↑                           ↑
   cut2 would exceed n         cut1 > k  or  cut1 > m

   lo = max(0, k-n)               hi = min(k, m)
```

---

### Concrete Boundary Check (Example)

```
arr1 (m=5): [2, 3, 6, 7, 9]
arr2 (n=4): [1, 4, 8, 10]
k = 5

lo = max(0, 5-4) = 1
hi = min(5, 5)   = 5

→ cut1 ∈ [1, 5],  cut2 ∈ [4, 0]
```

|cut1|cut2|cut2 valid?|cut1 valid?|Status|
|---|---|---|---|---|
|0|5|❌ arr2 has only 4|✅|Out of bounds|
|**1**|**4**|✅|✅|← lo (safe start)|
|3|2|✅|✅|Safe|
|**5**|**0**|✅|✅|← hi (safe end)|
|6|-1|❌ negative|❌|Out of bounds|

The range `[1, 5]` is exactly the safe window.

---

### One-Line Memory Hook

> **`hi`** = _"how much can cut1 take?"_ → limited by array size AND k **`lo`** = _"how little can cut1 take?"_ → must compensate when k is too large for arr2 alone

---

## ✅ Full Java Solution

```java
class Solution {
    public long kthElement(int arr1[], int arr2[], int m, int n, int k) {

        // Always binary search on the smaller array
        if (m > n) {
            return kthElement(arr2, arr1, n, m, k);
        }

        // cut1 range: ensure cut2 = k - cut1 stays within [0, n]
        int lo = Math.max(0, k - n);
        int hi = Math.min(k, m);

        while (lo <= hi) {
            int cut1 = (lo + hi) / 2;
            int cut2 = k - cut1;

            // Boundary sentinel values
            long maxLeft1  = (cut1 == 0) ? Long.MIN_VALUE : arr1[cut1 - 1];
            long minRight1 = (cut1 == m) ? Long.MAX_VALUE : arr1[cut1];
            long maxLeft2  = (cut2 == 0) ? Long.MIN_VALUE : arr2[cut2 - 1];
            long minRight2 = (cut2 == n) ? Long.MAX_VALUE : arr2[cut2];

            if (maxLeft1 > minRight2) {
                // cut1 too large → move left
                hi = cut1 - 1;

            } else if (maxLeft2 > minRight1) {
                // cut1 too small → move right
                lo = cut1 + 1;

            } else {
                // Valid partition found!
                return Math.max(maxLeft1, maxLeft2);
            }
        }

        return -1; // unreachable for valid inputs
    }
}
```

---

## 🔁 Alternative Approach — Recursive Elimination (O(log k))

### Idea

At each step, eliminate `k/2` elements that **cannot** be the k-th smallest:

- Compare `arr1[k/2 - 1]` and `arr2[k/2 - 1]`
- The array with the **smaller** value at `k/2 - 1` cannot contain the k-th element in those first `k/2` positions → eliminate them
- Reduce `k` by `k/2` and recurse

### Base Cases

```
if k == 1         → return min(arr1[0], arr2[0])
if arr1 is empty  → return arr2[k-1]
if arr2 is empty  → return arr1[k-1]
```

### Java Code

```java
class Solution {
    public long kthElement(int[] arr1, int[] arr2, int m, int n, int k) {
        return solve(arr1, arr2, 0, 0, m, n, k);
    }

    private long solve(int[] a, int[] b, int i, int j, int m, int n, int k) {
        // Base cases
        if (i == m) return b[j + k - 1];
        if (j == n) return a[i + k - 1];
        if (k == 1) return Math.min(a[i], b[j]);

        int half = k / 2;

        // Pick k/2-th element from each (guard against going out of bounds)
        int aVal = (i + half - 1 < m) ? a[i + half - 1] : Integer.MAX_VALUE;
        int bVal = (j + half - 1 < n) ? b[j + half - 1] : Integer.MAX_VALUE;

        if (aVal <= bVal) {
            // Eliminate first half elements from arr1
            return solve(a, b, i + half, j, m, n, k - half);
        } else {
            // Eliminate first half elements from arr2
            return solve(a, b, i, j + half, m, n, k - half);
        }
    }
}
```

---

## 📊 Step-by-Step Trace (Recursive)

### Input: `arr1 = [2, 3, 6, 7, 9]`, `arr2 = [1, 4, 8, 10]`, `k = 5`

```
Call 1: i=0, j=0, k=5
  half = 2
  aVal = arr1[1] = 3
  bVal = arr2[1] = 4
  aVal(3) ≤ bVal(4) → eliminate arr1[0..1] → i = 2

Call 2: i=2, j=0, k=3
  half = 1
  aVal = arr1[2] = 6
  bVal = arr2[0] = 1
  bVal(1) < aVal(6) → eliminate arr2[0] → j = 1

Call 3: i=2, j=1, k=2
  half = 1
  aVal = arr1[2] = 6
  bVal = arr2[1] = 4
  bVal(4) < aVal(6) → eliminate arr2[1] → j = 2

Call 4: i=2, j=2, k=1
  k == 1 → return min(arr1[2], arr2[2]) = min(6, 8) = 6 ✅
```

---

## ⚖️ Approach Comparison

||Binary Search (Partition)|Recursive Elimination|
|---|---|---|
|**Time**|O(log(min(m, n)))|O(log k)|
|**Space**|O(1)|O(log k) stack|
|**Intuition**|Partition-based|Elimination-based|
|**When k ≈ (m+n)/2**|Same as median problem|Slightly more steps|
|**Easier to remember**|⭐⭐⭐|⭐⭐⭐⭐|

---

## 🔑 Key Formulas Cheatsheet

```
lo   = max(0, k - n)
hi   = min(k, m)
cut1 = (lo + hi) / 2
cut2 = k - cut1

maxLeft1  = (cut1 == 0) ? -∞ : arr1[cut1 - 1]
minRight1 = (cut1 == m) ? +∞ : arr1[cut1]
maxLeft2  = (cut2 == 0) ? -∞ : arr2[cut2 - 1]
minRight2 = (cut2 == n) ? +∞ : arr2[cut2]

Valid when: maxLeft1 ≤ minRight2  AND  maxLeft2 ≤ minRight1

Answer = max(maxLeft1, maxLeft2)
```

---

## 🪤 Common Mistakes

|Mistake|Fix|
|---|---|
|Using `lo = 0, hi = m` without bounds check|Use `lo = max(0, k-n)`, `hi = min(k, m)`|
|Not swapping when `m > n`|Always binary search on the smaller array|
|Off-by-one on `cut` index|`arr1[cut1-1]` is the last included element on the left|
|Forgetting sentinel values|Use `MIN_VALUE` / `MAX_VALUE` when cut is at boundary|
|Returning `minRight` instead of `maxLeft`|Answer is `max` of left sides, not `min` of right sides|

---

## ⏱️ Complexity Analysis

### Binary Search (Partition)

|||
|---|---|
|**Time**|O(log(min(m, n)))|
|**Space**|O(1)|

### Recursive Elimination

|||
|---|---|
|**Time**|O(log k)|
|**Space**|O(log k) recursion stack|

---

## 🔗 Related Problems

- [[Median of Two Sorted Arrays]]
- [[Search in Rotated Sorted Array]]
- [[Kth Largest Element in an Array]]
- [[Find K Pairs with Smallest Sums]]