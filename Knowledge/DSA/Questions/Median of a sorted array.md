---

tags:

- dsa
- binary-search
- array
- hard
- leetcode
- divide-and-conquer
- two-pointers aliases:
- Median of Two Sorted Arrays
- LeetCode 4 difficulty: Hard topic: Binary Search pattern: Partition Array date: 2026-04-25

---

# Median of Two Sorted Arrays

## 📌 Problem Statement

> Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the **median** of the two sorted arrays.  
> The overall run time complexity must be **O(log(m+n))**.

**LeetCode #4** | Difficulty: 🔴 Hard

### Example

```
nums1 = [1, 3]
nums2 = [2]

Merged = [1, 2, 3]
Median  = 2.0
```

```
nums1 = [1, 2]
nums2 = [3, 4]

Merged = [1, 2, 3, 4]
Median  = (2 + 3) / 2 = 2.5
```

---

## 🧠 Intuition — What is a Median?

The **median** divides a sorted sequence into two equal halves:

- **Left half** ≤ all elements in **Right half**
- If total length is **odd** → median is the **middle element**
- If total length is **even** → median is **average of two middle elements**

### Key Insight

We don't need to merge the arrays.  
We just need to find the **correct partition** across both arrays such that:

```
left half of nums1  +  left half of nums2  =  right half of nums1  +  right half of nums2
```

---

## ✂️ The Partition Idea

Imagine splitting both arrays with a vertical cut:

```
nums1:  [ ... left1 | right1 ... ]
nums2:  [ ... left2 | right2 ... ]
```

A valid partition satisfies **two conditions**:

|Condition|Meaning|
|---|---|
|`len(left1) + len(left2) == len(right1) + len(right2)`|Equal halves (±1 for odd total)|
|`max(left1, left2) ≤ min(right1, right2)`|Left side ≤ Right side|

Once we find this partition, the median is:

- **Odd total:** `max(left1, left2)`
- **Even total:** `( max(left1, left2) + min(right1, right2) ) / 2.0`

---

## 📐 Pictorial Concept

### Setup (n = 4 example)

```
nums1 = [1, 3, 5, 7]     (m = 4)
nums2 = [2, 4, 6, 8]     (n = 4)

Total length = 8  →  each half should have 4 elements
```

### Binary Search on nums1's cut position

We binary search on the **smaller array** (say `nums1`) for a partition point `cut1`.  
`cut2` is then determined automatically:

```
cut2 = (m + n + 1) / 2 - cut1
```

#### Visual: Trying cut1 = 2

```
         cut1 = 2
              ↓
nums1:  [ 1  3 | 5  7 ]
         ←left1→ ←right1→

         cut2 = 2
              ↓
nums2:  [ 2  4 | 6  8 ]
         ←left2→ ←right2→

Left half:   { 1, 3, 2, 4 }   →  max = 4
Right half:  { 5, 7, 6, 8 }   →  min = 5

✅  4 ≤ 5  →  Valid partition!
Median = (4 + 5) / 2.0 = 4.5
```

#### Visual: Invalid partition (cut1 too small)

```
         cut1 = 1
              ↓
nums1:  [ 1 | 3  5  7 ]

         cut2 = 3
                   ↓
nums2:  [ 2  4  6 | 8 ]

Left half:   { 1, 2, 4, 6 }   →  max(left1, left2) = max(1, 6) = 6
Right half:  { 3, 5, 7, 8 }   →  min(right1, right2) = min(3, 8) = 3

❌  6 > 3  →  cut1 is too small → move cut1 RIGHT
```

#### Visual: Invalid partition (cut1 too large)

```
         cut1 = 3
                   ↓
nums1:  [ 1  3  5 | 7 ]

         cut2 = 1
              ↓
nums2:  [ 2 | 4  6  8 ]

Left half:   { 1, 3, 5, 2 }   →  max(left1, left2) = max(5, 2) = 5
Right half:  { 7, 4, 6, 8 }   →  min(right1, right2) = min(7, 4) = 4

❌  5 > 4  →  cut1 is too large → move cut1 LEFT
```

---

## 🔀 Binary Search Decision Logic

```
Binary search on cut1 ∈ [0, m]  (m = size of nums1)

At each step:
  maxLeft1  = nums1[cut1 - 1]   (or -∞ if cut1 == 0)
  minRight1 = nums1[cut1]       (or +∞ if cut1 == m)
  maxLeft2  = nums2[cut2 - 1]   (or -∞ if cut2 == 0)
  minRight2 = nums2[cut2]       (or +∞ if cut2 == n)

  if maxLeft1 > minRight2 → cut1 is too large → move LEFT  (hi = cut1 - 1)
  if maxLeft2 > minRight1 → cut1 is too small → move RIGHT (lo = cut1 + 1)
  else → FOUND ✅
```

---

## 🔢 Edge Cases: Boundary Values

When the cut is at the very edge of an array, we use sentinel values:

```
cut1 == 0  →  maxLeft1  = Integer.MIN_VALUE   (nothing on the left of nums1)
cut1 == m  →  minRight1 = Integer.MAX_VALUE   (nothing on the right of nums1)
cut2 == 0  →  maxLeft2  = Integer.MIN_VALUE
cut2 == n  →  minRight2 = Integer.MAX_VALUE
```

This ensures the boundary conditions work without special-casing them.

---

## ✅ Full Java Solution

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        // Always binary search on the smaller array for efficiency
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.length;
        int n = nums2.length;
        int lo = 0, hi = m;
        int half = (m + n + 1) / 2; // size of the left half

        while (lo <= hi) {
            int cut1 = (lo + hi) / 2;       // partition index in nums1
            int cut2 = half - cut1;          // partition index in nums2

            // Elements just left and right of the cut in nums1
            int maxLeft1  = (cut1 == 0) ? Integer.MIN_VALUE : nums1[cut1 - 1];
            int minRight1 = (cut1 == m) ? Integer.MAX_VALUE : nums1[cut1];

            // Elements just left and right of the cut in nums2
            int maxLeft2  = (cut2 == 0) ? Integer.MIN_VALUE : nums2[cut2 - 1];
            int minRight2 = (cut2 == n) ? Integer.MAX_VALUE : nums2[cut2];

            if (maxLeft1 > minRight2) {
                // cut1 is too far right → move left
                hi = cut1 - 1;

            } else if (maxLeft2 > minRight1) {
                // cut1 is too far left → move right
                lo = cut1 + 1;

            } else {
                // Valid partition found!
                int maxLeft  = Math.max(maxLeft1, maxLeft2);
                int minRight = Math.min(minRight1, minRight2);

                if ((m + n) % 2 == 1) {
                    return maxLeft;           // odd total → middle element
                } else {
                    return (maxLeft + minRight) / 2.0; // even total → average
                }
            }
        }

        return 0.0; // unreachable if inputs are valid sorted arrays
    }
}
```

---

## 📊 Step-by-Step Trace

### Input: `nums1 = [1, 3]`, `nums2 = [2, 4, 5]`

```
m = 2, n = 3, total = 5 (odd), half = 3
Binary search: lo = 0, hi = 2
```

|Iteration|cut1|cut2|maxLeft1|minRight1|maxLeft2|minRight2|Decision|
|---|---|---|---|---|---|---|---|
|1|1|2|1|3|2|4|maxLeft2(2) ≤ minRight1(3) ✅|

```
Valid partition!
maxLeft  = max(1, 2) = 2
minRight = min(3, 4) = 3

Total is odd → Median = maxLeft = 2  ✅
```

---

### Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`

```
m = 2, n = 2, total = 4 (even), half = 2
Binary search: lo = 0, hi = 2
```

|Iteration|cut1|cut2|maxLeft1|minRight1|maxLeft2|minRight2|Decision|
|---|---|---|---|---|---|---|---|
|1|1|1|1|2|3|4|maxLeft2(3) > minRight1(2) → move RIGHT|
|2|2|0|2|+∞|-∞|3|Valid ✅|

```
maxLeft  = max(2, -∞) = 2
minRight = min(+∞, 3) = 3

Total is even → Median = (2 + 3) / 2.0 = 2.5  ✅
```

---

## 🧩 Why Binary Search on the Smaller Array?

- `cut2 = half - cut1` must always be valid: `0 ≤ cut2 ≤ n`
- Searching on the smaller array (size `m`) ensures `cut2` stays in bounds
- Also gives us **O(log(min(m, n)))** time

---

## ⏱️ Complexity Analysis

|||
|---|---|
|**Time**|O(log(min(m, n))) — binary search on smaller array|
|**Space**|O(1) — no extra arrays used|

> ⚠️ Naive merge approach gives O(m+n) time — fails the constraint.

---

## 🔑 Key Formulas Cheatsheet

```
half    = (m + n + 1) / 2
cut2    = half - cut1

maxLeft1  = (cut1 == 0) ? -∞ : nums1[cut1 - 1]
minRight1 = (cut1 == m) ? +∞ : nums1[cut1]
maxLeft2  = (cut2 == 0) ? -∞ : nums2[cut2 - 1]
minRight2 = (cut2 == n) ? +∞ : nums2[cut2]

Valid when: maxLeft1 ≤ minRight2  AND  maxLeft2 ≤ minRight1

Median (odd)  = max(maxLeft1, maxLeft2)
Median (even) = ( max(maxLeft1, maxLeft2) + min(minRight1, minRight2) ) / 2.0
```

---

## 🪤 Common Mistakes

|Mistake|Fix|
|---|---|
|Binary searching on the larger array|Always swap so `nums1` is smaller|
|Integer overflow in `(maxLeft + minRight) / 2`|Cast to `double` before dividing|
|Forgetting sentinel values at boundaries|Use `MIN_VALUE` / `MAX_VALUE` when cut is at edge|
|Using `cut2 = n/2` instead of `half - cut1`|`cut2` must be derived from `cut1`, not independently|

---

## 🔗 Related Problems

- [[Binary Search]]
- [[Find Kth Smallest Element in Two Sorted Arrays]]
- [[Search in Rotated Sorted Array]]
- [[Kth Largest Element in an Array]]