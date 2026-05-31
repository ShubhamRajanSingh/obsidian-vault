## Problem Statement

You are given `n` stalls at positions stored in an array, and `k` cows to place in them.  
Place the cows such that the **minimum distance between any two cows is maximized**.

Return that **maximum possible minimum distance**.

> **Constraints:**
> 
> - `2 ≤ k ≤ n ≤ 10^5`
> - Stall positions can be unsorted

**Example:**

```
Stalls:  [1, 2, 4, 8, 9]
Cows:    3

Possible placements:
  [1, 2, 4] → min dist = 1
  [1, 2, 8] → min dist = 1
  [1, 4, 8] → min dist = 3  ✅
  [1, 4, 9] → min dist = 3  ✅
  [1, 8, 9] → min dist = 1
  [2, 4, 8] → min dist = 2
  [2, 8, 9] → min dist = 1
  [4, 8, 9] → min dist = 1

Answer = 3
```

---

## Key Insight: Binary Search on Answer

Instead of trying all placements, **binary search on the minimum distance** `d`:

- **Can we place all `k` cows such that every consecutive pair is at least `d` apart?**
- If YES → try a larger `d` (we can do better)
- If NO → try a smaller `d`

The answer is the **largest `d`** for which placement is feasible.

### Search Space

|Boundary|Value|Reason|
|---|---|---|
|`lo`|1|Minimum possible distance between any two stalls|
|`hi`|`max - min`|Maximum possible spread of stalls|

---

## Algorithm

### Step 1 — Sort the stalls

Binary search requires the stall positions to be in sorted order.

### Step 2 — Check feasibility for a given distance `d`

Greedily place cows:

- Place first cow at `stalls[0]`.
- For each subsequent stall, place the next cow only if the gap from the last placed cow is `≥ d`.
- If we place all `k` cows successfully → feasible.

### Step 3 — Binary search on `d`

- If `canPlace(d)` is true → `lo = mid + 1` (try larger)
- Else → `hi = mid - 1`
- Track the best valid `d` found.

---

## Code (Java)

```java
import java.util.Arrays;

public class AggressiveCows {

    // Check if we can place k cows with min distance >= d
    private static boolean canPlace(int[] stalls, int k, int d) {
        int count = 1;             // Place first cow at stalls[0]
        int lastPlaced = stalls[0];

        for (int i = 1; i < stalls.length; i++) {
            if (stalls[i] - lastPlaced >= d) {
                count++;
                lastPlaced = stalls[i];
                if (count == k) return true;
            }
        }

        return false;
    }

    public static int solve(int[] stalls, int k) {
        Arrays.sort(stalls);

        int lo = 1;
        int hi = stalls[stalls.length - 1] - stalls[0];
        int answer = 0;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;

            if (canPlace(stalls, k, mid)) {
                answer = mid;     // Valid placement found, try larger
                lo = mid + 1;
            } else {
                hi = mid - 1;     // Can't place, try smaller
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        int[] stalls = {1, 2, 4, 8, 9};
        int k = 3;
        System.out.println("Maximum minimum distance: " + solve(stalls, k)); // Output: 3
    }
}
```

---

## Dry Run

```
Stalls (sorted): [1, 2, 4, 8, 9]
k = 3
lo = 1, hi = 9 - 1 = 8

--- Iteration 1 ---
mid = 4
canPlace(d=4)?
  Place cow at stalls[0]=1, count=1, lastPlaced=1
  stalls[1]=2 → 2-1=1 < 4, skip
  stalls[2]=4 → 4-1=3 < 4, skip
  stalls[3]=8 → 8-1=7 ≥ 4 ✅, count=2, lastPlaced=8
  stalls[4]=9 → 9-8=1 < 4, skip
  count=2 < 3 → false
→ hi = 3

--- Iteration 2 ---
mid = 2
canPlace(d=2)?
  Place cow at 1, count=1, lastPlaced=1
  2-1=1 < 2, skip
  4-1=3 ≥ 2 ✅, count=2, lastPlaced=4
  8-4=4 ≥ 2 ✅, count=3 == k → true
→ answer = 2, lo = 3

--- Iteration 3 ---
mid = 3
canPlace(d=3)?
  Place cow at 1, count=1, lastPlaced=1
  2-1=1 < 3, skip
  4-1=3 ≥ 3 ✅, count=2, lastPlaced=4
  8-4=4 ≥ 3 ✅, count=3 == k → true
→ answer = 3, lo = 4

lo=4 > hi=3 → stop

Answer = 3 ✅
```

---

## Complexity

|||
|---|---|
|**Time**|O(n log n + n log(max − min))|
|**Space**|O(1)|

- Sorting: O(n log n)
- Binary search: O(log(max − min)) iterations × O(n) per feasibility check

---

## Edge Cases

|Scenario|Handling|
|---|---|
|`k = 2`|Always feasible; answer = `stalls[n-1] - stalls[0]`|
|`k = n`|One cow per stall; answer = min gap between adjacent stalls|
|All stalls same position|`hi = 0`; answer = 0|
|Unsorted input|Sorting in Step 1 handles it|
|Large stall values|`lo + (hi - lo) / 2` prevents integer overflow|

---

## Intuition: Why Greedy Works in `canPlace`?

Placing a cow as early as possible (greedily) never hurts:

- If we **skip** a valid stall, we make future placements harder.
- Placing **earlier** gives more room for remaining cows on the right.

This greedy choice is optimal — if a placement is feasible, the greedy check will always find it.

---

## Variations & Similar Problems

|Problem|Core Idea|
|---|---|
|Book Allocation|Binary search on max pages; check if `k` students can read|
|Painter's Partition|Binary search on max time; check if `k` painters can finish|
|Capacity to Ship Packages in D Days|Binary search on ship capacity|
|Koko Eating Bananas — LeetCode 875|Binary search on eating speed|
|Split Array Largest Sum — LeetCode 410|Binary search on answer|

> **Pattern:** Whenever a problem asks to **maximize the minimum** or **minimize the maximum**, think **Binary Search on Answer**.