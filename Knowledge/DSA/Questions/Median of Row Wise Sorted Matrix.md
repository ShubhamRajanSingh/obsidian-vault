
## Problem Statement

Given an `r × c` matrix where each row is sorted in ascending order, find the **median** of all elements in the matrix.

> **Constraints:**
> 
> - `r` and `c` are **odd** (ensures a unique median exists)
> - Matrix values are integers

**Example:**

```
Matrix:
1   3   5
2   6   9
3   6   9

All elements sorted: [1, 2, 3, 3, 5, 6, 6, 9, 9]
Median = 5  (index 4 in 0-based sorted array of 9 elements)
```

---

## Approach 1: Brute Force

### Intuition

Flatten the matrix into a 1D array, sort it, and return the middle element.

### Steps

1. Copy all elements into a list.
2. Sort the list.
3. Return element at index `(r * c) / 2`.

### Code (Java)

```java
import java.util.Arrays;

public class MedianBruteForce {
    public static int findMedian(int[][] matrix) {
        int r = matrix.length;
        int c = matrix[0].length;

        int[] flat = new int[r * c];
        int idx = 0;

        for (int[] row : matrix) {
            for (int val : row) {
                flat[idx++] = val;
            }
        }

        Arrays.sort(flat);
        return flat[(r * c) / 2];
    }

    public static void main(String[] args) {
        int[][] matrix = {
            {1, 3, 5},
            {2, 6, 9},
            {3, 6, 9}
        };
        System.out.println("Median: " + findMedian(matrix)); // Output: 5
    }
}
```

### Complexity

|||
|---|---|
|**Time**|O(r × c × log(r × c))|
|**Space**|O(r × c)|

---

## Approach 2: Binary Search on Value Range (Optimal)

### Intuition

The median is the element `m` such that **exactly `(r × c) / 2` elements are less than or equal to `m`** in the entire matrix.

Since each row is sorted, we can use **binary search on each row** to count how many elements are `≤ mid` in O(log c) time.

### Key Insight

For a candidate value `mid`:

- Count elements `≤ mid` across all rows using `upperBound()` (binary search).
- If this count `≤ (r × c) / 2`, the median lies to the **right** of `mid`.
- Otherwise, it lies to the **left**.

### Steps

1. Find `lo = min element` and `hi = max element` in the matrix.
2. Binary search on `mid = (lo + hi) / 2`.
3. For each row, count elements `≤ mid` using upper bound.
4. If total count `≤ n/2`, move `lo = mid + 1`.
5. Else, move `hi = mid`.
6. Return `lo` when `lo == hi`.

### Code (Java)

```java
public class MedianBinarySearch {

    // Count of elements <= val in a sorted row
    private static int upperBound(int[] row, int val) {
        int lo = 0, hi = row.length;
        while (lo < hi) {
            int mid = (lo + hi) / 2;
            if (row[mid] <= val) lo = mid + 1;
            else hi = mid;
        }
        return lo;
    }

    public static int findMedian(int[][] matrix) {
        int r = matrix.length;
        int c = matrix[0].length;
        int n = r * c;

        int lo = Integer.MAX_VALUE;
        int hi = Integer.MIN_VALUE;

        // Find global min and max
        for (int[] row : matrix) {
            lo = Math.min(lo, row[0]);
            hi = Math.max(hi, row[c - 1]);
        }

        int required = n / 2; // median index (0-based)

        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;

            // Count elements <= mid across all rows
            int count = 0;
            for (int[] row : matrix) {
                count += upperBound(row, mid);
            }

            if (count <= required) {
                lo = mid + 1;
            } else {
                hi = mid;
            }
        }

        return lo;
    }

    public static void main(String[] args) {
        int[][] matrix = {
            {1, 3, 5},
            {2, 6, 9},
            {3, 6, 9}
        };
        System.out.println("Median: " + findMedian(matrix)); // Output: 5
    }
}
```

### Complexity

|||
|---|---|
|**Time**|O(r × log c × log(max − min))|
|**Space**|O(1)|

---

## Dry Run (Binary Search Approach)

```
Matrix:
1  3  5
2  6  9
3  6  9

r=3, c=3, n=9, required = 9/2 = 4
lo=1, hi=9

--- Iteration 1 ---
mid = 5
upperBound row[0]={1,3,5}, val=5 → 3
upperBound row[1]={2,6,9}, val=5 → 1
upperBound row[2]={3,6,9}, val=5 → 1
count = 5 > 4 → hi = 5

--- Iteration 2 ---
mid = 3
upperBound row[0]={1,3,5}, val=3 → 2
upperBound row[1]={2,6,9}, val=3 → 1
upperBound row[2]={3,6,9}, val=3 → 1
count = 4 ≤ 4 → lo = 4

--- Iteration 3 ---
mid = 4
upperBound row[0]={1,3,5}, val=4 → 2
upperBound row[1]={2,6,9}, val=4 → 1
upperBound row[2]={3,6,9}, val=4 → 1
count = 4 ≤ 4 → lo = 5

lo == hi == 5 → Median = 5 ✅
```

---

## Edge Cases

|Scenario|Handling|
|---|---|
|All elements equal|`lo == hi` from start; binary search terminates immediately|
|Single row|Works as-is; `r=1`, applies upper bound on single row|
|Single column|Works as-is; each row has one element|
|Duplicate values|`upperBound` counts correctly with `<=` comparison|
|Large value range|`lo + (hi - lo) / 2` prevents integer overflow|

---

## Comparison of Approaches

|Approach|Time Complexity|Space Complexity|Notes|
|---|---|---|---|
|Brute Force|O(rc log rc)|O(rc)|Simple; not suitable for large matrices|
|Binary Search|O(r log c log(max−min))|O(1)|Optimal; works for large matrices|

---

## Similar Problems

- [Kth Smallest in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) — LeetCode 378
- [Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance/) — Binary search on answer
- Median of Two Sorted Arrays — LeetCode 4