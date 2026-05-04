# 239. Sliding Window Maximum

**Difficulty:** Hard **Topics:** Array, Queue, Sliding Window, Monotonic Deque **Link:** https://leetcode.com/problems/sliding-window-maximum/

---

## Problem Statement

You are given an array of integers `nums` and an integer `k`. There is a sliding window of size `k` moving from the very left to the very right of the array. You can only see the `k` numbers in the window. Each time the sliding window moves one position to the right, return the **maximum** of each window.

---

## Examples

### Example 1

```
Input:  nums = [1, 3, -1, -3, 5, 3, 6, 7], k = 3
Output: [3, 3, 5, 5, 6, 7]
```

```
Window position              Max
[1  3  -1] -3  5  3  6  7    3
 1 [3  -1  -3] 5  3  6  7    3
 1  3 [-1  -3  5] 3  6  7    5
 1  3  -1 [-3  5  3] 6  7    5
 1  3  -1  -3 [5  3  6] 7    6
 1  3  -1  -3  5 [3  6  7]   7
```

### Example 2

```
Input:  nums = [1], k = 1
Output: [1]
```

---

## Constraints

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `1 <= k <= nums.length`

---

## Intuition

A brute force approach scans every window of size `k` to find the max — O(n·k), which is too slow for large inputs.

The key observation is:

> If a new element entering the window is **greater than** elements already in the window, those smaller elements can **never be the maximum** for any future window — they can be discarded permanently.

This is the classic **Monotonic Decreasing Deque** pattern:

- The deque stores **actual values** in **decreasing order**.
- The **front** of the deque is always the **maximum** of the current window.
- When the window slides, we remove the outgoing element from the front (if it's there).
- Before adding a new element, we remove all smaller elements from the back.

---

## Data Structure: Monotonic Decreasing Deque

```
Deque stores values (not indices), decreasing from front to back.

After processing [1, 3, -1] with k=3:
  front → [3, -1] ← back
  max = peek front = 3 ✅

After sliding to [3, -1, -3]:
  Outgoing = arr[0] = 1 → not at front (3 ≠ 1), skip removal
  Incoming = -3 → smaller than -1, just add to back
  front → [3, -1, -3] ← back
  max = 3 ✅

After sliding to [-1, -3, 5]:
  Outgoing = arr[1] = 3 → IS at front (3 == 3), remove it
  Incoming = 5 → larger than everything, clear deque, add 5
  front → [5] ← back
  max = 5 ✅
```

---

## Solution Walkthrough

The solution has two phases:

### Phase 1 — Fill the first window (`i < k`)

Build the deque for indices `0` to `k-1`. Maintain decreasing order by removing smaller elements from the back before adding the new one.

```
When i == k-1 (last element of first window):
  res[0] = queue.peek()  → record first window's max
```

### Phase 2 — Slide the window (`i >= k`)

For each new element:

1. **Remove outgoing element** — If `queue.peek() == arr[i - k]`, the element leaving the window is at the front → poll it.
2. **Maintain decreasing order** — Remove all back elements smaller than `arr[i]`.
3. **Add new element** to the back.
4. **Record max** — `res[i - k + 1] = queue.peek()`

### Edge Case — `k >= arr.length`

If the window covers the entire array (or more), just return the global maximum directly. This avoids the sliding logic entirely.

---

## The Code

```java
class Solution {
    public int[] maxSlidingWindow(int[] arr, int k) {

        // Edge case: window covers entire array — return global max
        if (k >= arr.length) {
            int max = Integer.MIN_VALUE;
            for (int i = 0; i < arr.length; i++) {
                if (max < arr[i]) {
                    max = arr[i];
                }
            }
            return new int[]{max};
        }

        Deque<Integer> queue = new ArrayDeque<>(); // stores actual values (decreasing)
        int[] res = new int[arr.length - k + 1];   // result array

        for (int i = 0; i < arr.length; i++) {

            if (i < k) {
                // ── Phase 1: Build the first window ──────────────────────────
                // Remove back elements smaller than current (they're useless)
                while (!queue.isEmpty() && queue.peekLast() < arr[i]) {
                    queue.pollLast();
                }
                queue.add(arr[i]);

                // Once first window is complete, record its max
                if (i == k - 1) {
                    res[0] = queue.peek();
                }

            } else {
                // ── Phase 2: Slide the window ─────────────────────────────────

                // Step 1: Remove outgoing element (arr[i - k]) if it's at the front
                if (queue.peek() == arr[i - k]) {
                    queue.poll();
                }

                // Step 2: Remove back elements smaller than incoming element
                while (!queue.isEmpty() && queue.peekLast() < arr[i]) {
                    queue.pollLast();
                }

                // Step 3: Add new element to back
                queue.add(arr[i]);

                // Step 4: Front of deque is the current window's max
                res[i - k + 1] = queue.peek();
            }
        }

        return res;
    }
}
```

---

## Dry Run

**Input:** `arr = [1, 3, -1, -3, 5, 3, 6, 7]`, `k = 3`

**Phase 1 — Build first window (i = 0, 1, 2):**

|i|arr[i]|Action|Deque (front→back)|res|
|---|---|---|---|---|
|0|1|add 1|[1]|—|
|1|3|3 > 1 → remove 1, add 3|[3]|—|
|2|-1|-1 < 3 → just add|[3, -1]|res[0]=**3**|

**Phase 2 — Slide window (i = 3 to 7):**

|i|arr[i]|Outgoing arr[i-k]|Remove front?|Deque after remove|Add arr[i]|Deque final|res[i-k+1]|
|---|---|---|---|---|---|---|---|
|3|-3|arr[0]=1|3≠1 → No|[3,-1]|-3<-1, add|[3,-1,-3]|res[1]=**3**|
|4|5|arr[1]=3|3==3 → Yes|[-1,-3]|5>all, clear, add|[5]|res[2]=**5**|
|5|3|arr[2]=-1|5≠-1 → No|[5]|3<5, add|[5,3]|res[3]=**5**|
|6|6|arr[3]=-3|5≠-3 → No|[5,3]|6>all, clear, add|[6]|res[4]=**6**|
|7|7|arr[4]=5|6≠5 → No|[6]|7>6, clear, add|[7]|res[5]=**7**|

**Output:** `[3, 3, 5, 5, 6, 7]` ✅

---

## A Note on Storing Values vs Indices

This solution stores **actual values** in the deque. This works correctly but has one subtle implication:

> The outgoing check `queue.peek() == arr[i - k]` uses **value equality**.

This can fail when **duplicate values** exist. Consider:

```
arr = [3, 3, 1], k = 2
Window [3, 3]: deque = [3], max = 3 ✅
Slide to [3, 1]:
  Outgoing = arr[0] = 3
  queue.peek() = 3 → match → poll
  Deque = [], add 1 → [1]
  max = 1 ❌  (should be 3, the second 3 is still in window)
```

The standard production fix is to **store indices** instead of values:

```java
// Store indices; values are still compared via arr[index]
// Front removal: if (queue.peek() == i - k) queue.poll();   ← index comparison (always unique)
// Back removal:  while (!queue.isEmpty() && arr[queue.peekLast()] < arr[i]) queue.pollLast();
// Max:           arr[queue.peek()]
```

Index-based removal is safe because **indices are always unique**, avoiding false matches on equal values.

---

## Index-Based Solution (Handles Duplicates Correctly)

```java
class Solution {
    public int[] maxSlidingWindow(int[] arr, int k) {
        int n = arr.length;
        int[] res = new int[n - k + 1];
        Deque<Integer> deque = new ArrayDeque<>(); // stores indices

        for (int i = 0; i < n; i++) {
            // Remove indices outside the current window
            if (!deque.isEmpty() && deque.peek() == i - k) {
                deque.poll();
            }

            // Remove back indices whose values are smaller than current
            while (!deque.isEmpty() && arr[deque.peekLast()] < arr[i]) {
                deque.pollLast();
            }

            deque.add(i);

            // Start recording results once the first window is complete
            if (i >= k - 1) {
                res[i - k + 1] = arr[deque.peek()];
            }
        }

        return res;
    }
}
```

This version is cleaner — no separate phase 1/phase 2 split, no edge case guard needed, and handles duplicates correctly.

---

## Complexity Analysis

|Approach|Time|Space|
|---|---|---|
|Brute Force|O(n·k)|O(1)|
|Monotonic Deque (value-based)|O(n)|O(k)|
|Monotonic Deque (index-based)|O(n)|O(k)|

- **Time O(n):** Each element is added to the deque once and removed at most once → 2n operations total.
- **Space O(k):** The deque holds at most `k` elements at any point (one full window).

---

## Common Mistakes to Avoid

1. **Storing values instead of indices** — Works for most cases but breaks on duplicate values during the outgoing element check. Prefer index-based approach in interviews.
2. **Wrong result array size** — It should be `n - k + 1`, not `n` or `n - k`.
3. **Wrong outgoing index** — The element leaving the window at step `i` is `arr[i - k]`, not `arr[i - k + 1]`.
4. **Not handling `k == 1`** — Works naturally with the deque approach; every element is its own window max.
5. **Using `<` vs `<=` in back removal** — Using `<=` (strict decrease) vs `<` (non-strict) affects duplicate handling. `<` keeps duplicates in the deque which is safer.

---

## Related Problems

| #    | Problem                                   | Relationship                          |
| ---- | ----------------------------------------- | ------------------------------------- |
| 239  | **Sliding Window Maximum** ← You are here | Monotonic decreasing deque            |
| 84   | Largest Rectangle in Histogram            | Monotonic increasing stack            |
| 42   | Trapping Rain Water                       | Monotonic stack                       |
| 862  | Shortest Subarray with Sum at Least K     | Monotonic deque on prefix sums        |
| 1438 | Longest Subarray with Abs Diff ≤ Limit    | Two deques (max + min simultaneously) |