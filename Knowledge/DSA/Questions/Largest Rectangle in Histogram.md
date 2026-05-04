# Largest Rectangle in Histogram

**Difficulty:** Hard **Topics:** Array, Stack, Monotonic Stack **Link:** https://leetcode.com/problems/largest-rectangle-in-histogram/

---

## Problem Statement

Given an array of integers `heights` representing the histogram's bar heights where the width of each bar is `1`, return the **area of the largest rectangle** in the histogram.

---

## Examples

### Example 1

```
Input:  heights = [2, 1, 5, 6, 2, 3]
Output: 10
```

```
      ██
   ████
   ████
██ ████ ██
██ ████████
██████████ ██
 2  1  5  6  2  3

Largest rectangle: height=5, width=2 → area = 10
(bars at index 2 and 3)
```

### Example 2

```
Input:  heights = [2, 4]
Output: 4
```

---

## Constraints

- `1 <= heights.length <= 10^5`
- `0 <= heights[i] <= 10^4`

---

## Intuition

For every bar `i`, the largest rectangle that **uses bar `i` as its shortest bar** extends:

- **Left** → until a bar shorter than `heights[i]` is found
- **Right** → until a bar shorter than `heights[i]` is found

```
Area using bar i as height = heights[i] × (right_boundary - left_boundary - 1)
```

The brute force way is to check all pairs — O(n²). The key insight is:

> A **monotonic increasing stack** lets us find the left and right boundary for every bar in **amortized O(1)**, giving O(n) overall.

---

## Why a Monotonic Increasing Stack?

We maintain a stack of indices in **increasing order of heights**.

- When we encounter a bar **shorter** than the stack's top, the top bar has found its **right boundary** (the current bar).
- The new stack top after popping gives the **left boundary**.
- We compute the area and pop. Repeat until the stack top is ≤ current bar.

This way, every bar is pushed and popped **exactly once** → O(n).

---

## Step-by-Step Algorithm

1. Initialize an empty stack and `maxArea = 0`.
2. Iterate `i` from `0` to `n` (include one extra iteration with a sentinel height `0`).
3. While stack is not empty **and** `heights[i] < heights[stack.top()]`:
    - Pop the top index `top`.
    - Height = `heights[top]`
    - Width = `i - stack.top() - 1` (if stack is empty, width = `i`)
    - Update `maxArea`
4. Push `i` onto the stack.
5. Return `maxArea`.

> **Sentinel trick:** Appending a `0` at the end forces all remaining bars in the stack to be processed — no separate cleanup loop needed.

---

## Visual Trace

**Input:** `[2, 1, 5, 6, 2, 3]` (index 6 is sentinel `0`)

|i|h[i]|Stack (indices)|Action|Area computed|
|---|---|---|---|---|
|0|2|[0]|push|—|
|1|1|[1]|pop 0 → h=2, w=1 → area=2; push 1|2|
|2|5|[1,2]|push|—|
|3|6|[1,2,3]|push|—|
|4|2|[1,2,3] → pop|pop 3 → h=6, w=1 → area=6|6|
|||[1,2] → pop|pop 2 → h=5, w=2 → area=10|**10**|
|||[1]|push 4|—|
|5|3|[1,4,5]|push|—|
|6|0|[1,4,5] → pop|pop 5 → h=3, w=1 → area=3|3|
|||[1,4] → pop|pop 4 → h=2, w=3 → area=6|6|
|||[1] → pop|pop 1 → h=1, w=6 → area=6|6|
|||[]|push 6|—|

**maxArea = 10** ✅

---

## Width Calculation Explained

When we pop index `top`:

- **Right boundary** = current index `i` (the bar that triggered the pop — it is shorter)
- **Left boundary** = new `stack.top()` after the pop (the first bar to the left that is shorter)
- **Width** = `i - stack.top() - 1`

```
Example: pop index 2 (height=5) when i=4 (height=2)
  Stack after pop: top = index 1 (height=1)
  Width = 4 - 1 - 1 = 2   (indices 2 and 3 are both ≥ 5)
  Area  = 5 × 2 = 10
```

If the stack is **empty** after popping, it means the popped bar's height extends all the way to the left edge:

```
  Width = i - 0 = i   (spans from index 0 to i-1)
```

---

## Java Solution

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int maxArea = 0;
        Deque<Integer> stack = new ArrayDeque<>(); // stores indices

        for (int i = 0; i <= n; i++) {
            // Sentinel: treat end as height 0 to flush the stack
            int currHeight = (i == n) ? 0 : heights[i];

            while (!stack.isEmpty() && currHeight < heights[stack.peek()]) {
                int top = stack.pop();
                int height = heights[top];
                int width = stack.isEmpty() ? i : i - stack.peek() - 1;
                maxArea = Math.max(maxArea, height * width);
            }

            stack.push(i);
        }

        return maxArea;
    }
}
```

---

## Python Solution

```python
class Solution:
    def largestRectangleArea(self, heights: list[int]) -> int:
        max_area = 0
        stack = []  # stores indices

        for i, h in enumerate(heights + [0]):  # append sentinel 0
            while stack and h < heights[stack[-1]]:
                top = stack.pop()
                height = heights[top]
                width = i if not stack else i - stack[-1] - 1
                max_area = max(max_area, height * width)
            stack.append(i)

        return max_area
```

---

## Complexity Analysis

||Time|Space|
|---|---|---|
|Monotonic Stack|O(n)|O(n)|

- **Time:** Each bar is pushed once and popped once → 2n operations total → O(n).
- **Space:** Stack holds at most n indices → O(n).

---

## Brute Force (for contrast) — O(n²)

For every pair `(i, j)`, find the minimum height in that range and compute area:

```java
// O(n²) — TLE on large inputs
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length, maxArea = 0;
        for (int i = 0; i < n; i++) {
            int minH = heights[i];
            for (int j = i; j < n; j++) {
                minH = Math.min(minH, heights[j]);
                maxArea = Math.max(maxArea, minH * (j - i + 1));
            }
        }
        return maxArea;
    }
}
```

---

## Alternative: Precompute Left/Right Boundaries — O(n)

Instead of processing on-the-fly, use two passes to precompute:

- `left[i]` = index of the first bar to the left that is **strictly shorter** than `heights[i]`
- `right[i]` = index of the first bar to the right that is **strictly shorter** than `heights[i]`

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Deque<Integer> stack = new ArrayDeque<>();

        // Fill left boundaries
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && heights[stack.peek()] >= heights[i])
                stack.pop();
            left[i] = stack.isEmpty() ? -1 : stack.peek();
            stack.push(i);
        }

        stack.clear();

        // Fill right boundaries
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && heights[stack.peek()] >= heights[i])
                stack.pop();
            right[i] = stack.isEmpty() ? n : stack.peek();
            stack.push(i);
        }

        // Compute max area
        int maxArea = 0;
        for (int i = 0; i < n; i++) {
            int width = right[i] - left[i] - 1;
            maxArea = Math.max(maxArea, heights[i] * width);
        }

        return maxArea;
    }
}
```

Both approaches are O(n) time and O(n) space. The single-pass sentinel approach is more concise; the two-pass approach is easier to reason about.

---

## Common Mistakes to Avoid

1. **Wrong width formula** — Width is `i - stack.peek() - 1`, not `i - stack.peek()`. The `-1` accounts for the fact that both boundaries are exclusive.
2. **Forgetting the empty stack case** — When the stack is empty after popping, width = `i` (the bar spans the entire left side).
3. **Not flushing the stack at the end** — Without a sentinel `0`, bars remaining in the stack after the loop are never processed. Always append `0` or add a separate cleanup loop.
4. **Using `>=` instead of `<` for pop condition** — Pop when `currHeight < heights[top]`. Using `<=` may evict bars of equal height prematurely (though it still produces correct area, it breaks the invariant cleanly).

---

## Related Problems

|#|Problem|Relationship|
|---|---|---|
|85|Maximal Rectangle|Applies this exact solution row by row on a 2D matrix|
|42|Trapping Rain Water|Same monotonic stack direction, different computation|
|739|Daily Temperatures|Classic monotonic stack warmup|
|901|Online Stock Span|Monotonic stack for spans|