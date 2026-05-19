# 85. Maximal Rectangle

**Difficulty:** Hard **Topics:** Array, Stack, Dynamic Programming, Matrix, Monotonic Stack **Link:** https://leetcode.com/problems/maximal-rectangle/

---

## Prerequisite

This problem directly reduces to **LC 84 — Largest Rectangle in Histogram**. If you haven't read that solution yet, do so first — the histogram algorithm is used as a black box here.

---

## Problem Statement

Given a `rows × cols` binary matrix filled with `'0'`s and `'1'`s, find the largest rectangle containing only `'1'`s and return its **area**.

```
Input:
matrix = [
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]

Output: 6
```

---

## Examples

### Example 1

```
  1 0 1 0 0
  1 0 1 1 1
  1 1 1 1 1   ← largest rectangle here (2×3 or 1×6 = 6)
  1 0 0 1 0

Output: 6
```

### Example 2

```
Input:  matrix = [["0"]]
Output: 0
```

### Example 3

```
Input:  matrix = [["1"]]
Output: 1
```

---

## Constraints

- `rows == matrix.length`
- `cols == matrix[i].length`
- `1 <= rows, cols <= 200`
- `matrix[i][j]` is `'0'` or `'1'`

---

## Core Idea — Reduce to Histogram

For each row of the matrix, treat the **column heights** (number of consecutive `1`s ending at that row) as a **histogram**. Then apply the **Largest Rectangle in Histogram** algorithm on each row's histogram.

```
Row 0:  heights = [1, 0, 1, 0, 0]
Row 1:  heights = [2, 0, 2, 1, 1]
Row 2:  heights = [3, 1, 3, 2, 2]
Row 3:  heights = [4, 0, 0, 3, 0]
```

For each row's histogram, find the largest rectangle. The global maximum is the answer.

### Height Update Rule

```
If matrix[i][j] == '1':
    heights[j] = heights[j] + 1   ← extend the column upward

If matrix[i][j] == '0':
    heights[j] = 0                 ← column breaks here, reset to 0
```

---

## Visual Walkthrough

```
Matrix:
  1 0 1 0 0
  1 0 1 1 1
  1 1 1 1 1
  1 0 0 1 0

Row 0: heights = [1, 0, 1, 0, 0]
  █   █
  Largest histogram rect = 1

Row 1: heights = [2, 0, 2, 1, 1]
  █   █
  █   █ █ █
  Largest histogram rect = 3  (cols 2,3,4 → height=1, width=3)

Row 2: heights = [3, 1, 3, 2, 2]
  █   █
  █   █ █ █
  █ █ █ █ █
  Largest histogram rect = 6  (cols 2,3,4 → height=2, width=3 OR col 0..4 height=1 width=5=5)
  Actually: cols 2,3,4 → min height=2, width=3 → area=6 ✅

Row 3: heights = [4, 0, 0, 3, 0]
  █
  █     █
  █     █
  █     █
  Largest histogram rect = 4  (col 0, height=4)

Global max = 6  ✅
```

---

## Algorithm

1. Initialize `heights[cols] = {0}` — all zeros.
2. For each row `i`:
    - Update `heights[j]`: if `matrix[i][j] == '1'` → `heights[j]++`, else `heights[j] = 0`
    - Apply **Largest Rectangle in Histogram** on current `heights[]`
    - Update `maxArea`
3. Return `maxArea`

---

## Java Solution

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix == null || matrix.length == 0) return 0;

        int cols = matrix[0].length;
        int[] heights = new int[cols]; // histogram heights per column
        int maxArea = 0;

        for (char[] row : matrix) {
            // Step 1: Update heights for this row
            for (int j = 0; j < cols; j++) {
                heights[j] = (row[j] == '1') ? heights[j] + 1 : 0;
            }

            // Step 2: Apply LC 84 — Largest Rectangle in Histogram
            maxArea = Math.max(maxArea, largestRectangleInHistogram(heights));
        }

        return maxArea;
    }

    private int largestRectangleInHistogram(int[] heights) {
        int n = heights.length;
        int maxArea = 0;
        Deque<Integer> stack = new ArrayDeque<>(); // monotonic increasing stack of indices

        for (int i = 0; i <= n; i++) {
            int currHeight = (i == n) ? 0 : heights[i]; // sentinel 0 at end

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

## Full Dry Run

**Matrix:**

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

### Row 0 — `["1","0","1","0","0"]`

```
heights update:
  j=0: '1' → 0+1=1
  j=1: '0' → 0
  j=2: '1' → 0+1=1
  j=3: '0' → 0
  j=4: '0' → 0

heights = [1, 0, 1, 0, 0]

Histogram (with sentinel):
i=0: push 0         stack=[0]
i=1: h=0 < h[0]=1 → pop 0: h=1, stack empty, w=1 → area=1; push 1   stack=[1]
i=2: h=1 > h[1]=0 → push 2   stack=[1,2]
i=3: h=0 < h[2]=1 → pop 2: h=1, top=1, w=3-1-1=1 → area=1; push 3  stack=[1,3]
i=4: h=0 == h[3]=0 → push 4  stack=[1,3,4]
i=5(sentinel): h=0
  pop 4: h=0, w=... → area=0
  pop 3: h=0, w=... → area=0
  pop 1: h=0, w=... → area=0

maxArea after row 0 = 1
```

### Row 1 — `["1","0","1","1","1"]`

```
heights update:
  j=0: '1' → 1+1=2
  j=1: '0' → 0
  j=2: '1' → 1+1=2
  j=3: '1' → 0+1=1
  j=4: '1' → 0+1=1

heights = [2, 0, 2, 1, 1]

Key histogram trace:
  Largest rect = cols 2,3,4 → min height=1, width=3 → area=3
  Or col 2 alone → height=2, width=1 → area=2

maxArea after row 1 = 3
```

### Row 2 — `["1","1","1","1","1"]`

```
heights update:
  j=0: '1' → 2+1=3
  j=1: '1' → 0+1=1
  j=2: '1' → 2+1=3
  j=3: '1' → 1+1=2
  j=4: '1' → 1+1=2

heights = [3, 1, 3, 2, 2]

Histogram trace (with sentinel at i=5):
i=0: push 0         stack=[0]        h=[3]
i=1: h=1 < h[0]=3 → pop 0: h=3, stack empty, w=1 → area=3; push 1
     stack=[1]
i=2: h=3 > h[1]=1 → push 2   stack=[1,2]
i=3: h=2 < h[2]=3 → pop 2: h=3, top=1, w=3-1-1=1 → area=3; push 3
     stack=[1,3]
i=4: h=2 == h[3]=2 → push 4  stack=[1,3,4]
i=5(sentinel): h=0
  pop 4: h=2, top=3, w=5-3-1=1 → area=2
  pop 3: h=2, top=1, w=5-1-1=3 → area=6  ✅ ← NEW MAX
  pop 1: h=1, stack empty, w=5 → area=5
  push 5

maxArea after row 2 = 6
```

### Row 3 — `["1","0","0","1","0"]`

```
heights update:
  j=0: '1' → 3+1=4
  j=1: '0' → 0
  j=2: '0' → 0
  j=3: '1' → 2+1=3
  j=4: '0' → 0

heights = [4, 0, 0, 3, 0]

Largest rect = col 0 alone → height=4, width=1 → area=4
  or col 3 alone → height=3 → area=3

maxArea after row 3 = 6  (no improvement)
```

**Final Answer: 6** ✅

---

## Complexity Analysis

|Step|Time|Space|
|---|---|---|
|Update heights per row|O(cols)|O(cols)|
|Histogram per row|O(cols)|O(cols)|
|All rows combined|**O(rows × cols)**|**O(cols)**|

- **Time:** For each of the `rows` rows, we do O(cols) work → O(rows × cols) total.
- **Space:** Only `heights[]` and the stack → O(cols).

---

## Alternative — DP Approach

Instead of reducing to histogram, compute three arrays per row:

- `height[j]` = consecutive `1`s above (including) current row at column `j`
- `left[j]` = leftmost column where a rectangle of height `height[j]` can start
- `right[j]` = rightmost column where it can end

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        int rows = matrix.length, cols = matrix[0].length;
        int[] height = new int[cols];
        int[] left   = new int[cols];
        int[] right  = new int[cols];
        Arrays.fill(right, cols);

        int maxArea = 0;

        for (int i = 0; i < rows; i++) {
            int curLeft = 0, curRight = cols;

            // Update height
            for (int j = 0; j < cols; j++)
                height[j] = (matrix[i][j] == '1') ? height[j] + 1 : 0;

            // Update left boundary
            for (int j = 0; j < cols; j++) {
                if (matrix[i][j] == '1') left[j] = Math.max(left[j], curLeft);
                else { left[j] = 0; curLeft = j + 1; }
            }

            // Update right boundary
            for (int j = cols - 1; j >= 0; j--) {
                if (matrix[i][j] == '1') right[j] = Math.min(right[j], curRight);
                else { right[j] = cols; curRight = j; }
            }

            // Compute area
            for (int j = 0; j < cols; j++)
                maxArea = Math.max(maxArea, height[j] * (right[j] - left[j]));
        }

        return maxArea;
    }
}
```

||Histogram Approach|DP Approach|
|---|---|---|
|Time|O(rows × cols)|O(rows × cols)|
|Space|O(cols)|O(cols)|
|Intuition|Reuse LC 84 cleanly|Self-contained but harder to derive|
|Interview preference|✅ Cleaner, builds on known|Good follow-up|

---

## Common Mistakes to Avoid

1. **Not resetting `heights[j] = 0` on `'0'`** — If a cell is `'0'`, the column streak breaks. Forgetting to reset means heights carry over incorrectly from previous rows.
    
2. **Applying histogram on the raw matrix row** — The histogram input is `heights[]` (cumulative consecutive `1`s), not the raw `'0'`/`'1'` characters.
    
3. **Reinventing histogram logic** — Extract it into a helper method and reuse. Mixing histogram logic into the main loop makes the code harder to reason about.
    
4. **Off-by-one in sentinel** — The sentinel `0` is added at index `n` (one past the end). Loop must go `i <= n`, not `i < n`.
    
5. **Initializing `heights` to non-zero** — `heights[]` must start as all zeros. Any leftover value from a previous run (if reusing arrays) would corrupt the height computation.
    

---

## The Reduction Chain

```
Maximal Rectangle (2D)
       ↓  for each row, build heights[]
Largest Rectangle in Histogram (1D)
       ↓  monotonic increasing stack
O(cols) per row → O(rows × cols) total
```

This reduction is the key insight. Once you see that each row's cumulative heights form a histogram, the rest is mechanical application of LC 84.

---

## Related Problems

|#|Problem|Relationship|
|---|---|---|
|LC 84|Largest Rectangle in Histogram|Direct prerequisite — used as subroutine|
|LC 85|**Maximal Rectangle** ← You are here|Reduce to LC 84 row by row|
|LC 221|Maximal Square|Simpler DP — find largest square of 1s|
|LC 42|Trapping Rain Water|Monotonic stack on histogram|
|LC 1277|Count Square Submatrices|Count all squares of 1s|