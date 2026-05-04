# 994. Rotting Oranges

**Difficulty:** Medium **Topics:** Array, BFS, Matrix **Link:** https://leetcode.com/problems/rotting-oranges/

---

## Problem Statement

You are given an `m x n` grid where each cell can have one of three values:

- `0` — Empty cell
- `1` — Fresh orange
- `2` — Rotten orange

Every minute, any fresh orange **4-directionally adjacent** to a rotten orange becomes rotten.

Return the **minimum number of minutes** that must elapse until no cell has a fresh orange. If it is **impossible**, return `-1`.

---

## Examples

### Example 1

```
Input:
 2  1  1
 1  1  0
 0  1  1

Output: 4
```

```
Minute 0:   2  1  1       Minute 1:   2  2  1       Minute 2:   2  2  2
            1  1  0                   2  1  0                   2  2  0
            0  1  1                   0  1  1                   0  1  1

Minute 3:   2  2  2       Minute 4:   2  2  2
            2  2  0                   2  2  0
            0  2  1                   0  2  2   ← all rotten
```

### Example 2

```
Input:
 2  1  1
 0  1  1
 1  0  1

Output: -1   (bottom-left fresh orange is unreachable)
```

### Example 3

```
Input:  [[0, 2]]
Output: 0   (no fresh oranges to rot)
```

---

## Constraints

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` is `0`, `1`, or `2`

---

## Intuition

This is a **multi-source BFS** problem.

The key insight is that all rotten oranges spread **simultaneously** each minute — not one at a time. This is exactly what BFS models naturally: all nodes at the current "level" (minute) are processed before moving to the next level.

### Why BFS, not DFS?

- DFS explores one path deeply — it would rot oranges sequentially, not simultaneously.
- BFS explores level by level — every rotten orange at minute `t` spreads at the same time, matching the problem's simultaneous spread semantics.
- The BFS level count directly maps to the elapsed minutes.

### Multi-source BFS

Instead of starting BFS from a single source, we enqueue **all initially rotten oranges at once**. They all begin spreading at minute 0, which correctly models simultaneous propagation from multiple starting points.

```
Initial queue: [all cells with value 2]
Each BFS level = 1 minute elapsed
Stop when queue is empty
```

---

## Algorithm

1. **Scan the grid** — Enqueue all rotten oranges; count all fresh oranges (`freshCounter`).
2. **Early exit** — If `freshCounter == 0`, return `0` (nothing to rot).
3. **BFS level by level** — Each level represents one minute:
    - For each rotten orange in the current level, check all 4 neighbours.
    - If a neighbour is fresh (`1`), mark it rotten (`2`), enqueue it, decrement `freshCounter`.
    - Increment `time` once per level (only if at least one orange was rotted).
4. **After BFS** — If `freshCounter == 0`, return `time`. Otherwise return `-1`.

---

## Clean Solution

```java
import java.util.*;

class Solution {
    public int orangesRotting(int[][] grid) {
        int rows = grid.length, cols = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int freshCounter = 0;

        // Step 1: Enqueue all rotten oranges, count fresh ones
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 2) queue.add(new int[]{i, j});
                else if (grid[i][j] == 1) freshCounter++;
            }
        }

        // Step 2: No fresh oranges — nothing to do
        if (freshCounter == 0) return 0;

        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int time = 0;

        // Step 3: BFS level by level (each level = 1 minute)
        while (!queue.isEmpty()) {
            int size = queue.size();
            boolean rotted = false;

            for (int i = 0; i < size; i++) {
                int[] curr = queue.poll();

                for (int[] dir : directions) {
                    int ni = curr[0] + dir[0];
                    int nj = curr[1] + dir[1];

                    if (ni >= 0 && ni < rows && nj >= 0 && nj < cols
                            && grid[ni][nj] == 1) {
                        grid[ni][nj] = 2;       // mark rotten
                        queue.add(new int[]{ni, nj});
                        freshCounter--;
                        rotted = true;
                    }
                }
            }

            if (rotted) time++; // only count this minute if something rotted
        }

        // Step 4: If fresh oranges remain, they are unreachable
        return freshCounter == 0 ? time : -1;
    }
}
```

---

## Dry Run

**Input:**

```
2  1  1
1  1  0
0  1  1
```

`freshCounter = 6`, initial queue: `[(0,0)]`

|Minute|Queue (start of level)|Oranges rotted this minute|freshCounter|
|---|---|---|---|
|1|[(0,0)]|(0,1), (1,0)|4|
|2|[(0,1),(1,0)]|(0,2), (1,1)|2|
|3|[(0,2),(1,1)]|(2,1)|1|
|4|[(2,1)]|(2,2)|0|

`freshCounter == 0` → return `4` ✅

---

## Why `if (rotted) time++` Instead of Always `time++`?

Consider a grid where fresh oranges exist but none are reachable from any rotten orange:

```
Grid: [[1, 2]]   — fresh at (0,0), rotten at (0,1)

Minute 1: rotten at (0,1) spreads to (0,0) → freshCounter = 0, rotted = true → time = 1
```

But what if the queue is not empty yet no orange gets rotted in a level? That shouldn't count as a minute passing. The `rotted` flag ensures `time` only increments when actual spreading occurred.

Alternatively — and more cleanly — this is avoided entirely by tracking **timestamps** via storing `(row, col, time)` in the node, or simply trusting that if nothing rots in a level, the while loop ends naturally. The `rotted` flag is a safe guard.

---

## Edge Cases

|Scenario|Expected Output|Why|
|---|---|---|
|No fresh oranges|`0`|Early exit after scan|
|No rotten oranges, fresh exist|`-1`|BFS never runs, freshCounter > 0|
|All cells empty (`0`)|`0`|freshCounter stays 0|
|Single rotten orange, surrounded by fresh|BFS handles naturally|Multi-spread per level|
|Disconnected fresh orange|`-1`|freshCounter never reaches 0|

---

## Complexity Analysis

||Time|Space|
|---|---|---|
|BFS|O(m × n)|O(m × n)|

- **Time:** Every cell is enqueued and processed at most once.
- **Space:** Queue holds at most O(m × n) nodes.

---

## Common Mistakes to Avoid

1. **Using DFS instead of BFS** — DFS cannot model simultaneous spread; it would overcount time.
2. **Not doing multi-source BFS** — Starting BFS from one rotten orange at a time gives wrong time for grids with multiple initial rotten oranges.
3. **Counting time even when nothing rots** — If a BFS level processes nodes but no fresh orange is adjacent, that minute shouldn't count. Use a `rotted` flag or timestamp approach.
4. **Mutating the grid without marking visited** — Marking `grid[ni][nj] = 2` as soon as it's enqueued (not when dequeued) is crucial. Without this, the same fresh orange can be enqueued multiple times by different rotten neighbours.
5. **Forgetting the `freshCounter == 0` early exit** — Without it, a grid of all rotten/empty cells would still run BFS and return `0` eventually, but the early exit makes the intent explicit and handles `[[2]]` cleanly.

---

## Related Problems

|#|Problem|Relationship|
|---|---|---|
|994|**Rotting Oranges** ← You are here|Multi-source BFS on grid|
|542|01 Matrix|Multi-source BFS (distance to nearest 0)|
|1162|As Far from Land as Possible|Multi-source BFS (max distance)|
|286|Walls and Gates|Multi-source BFS (fill distances)|
|127|Word Ladder|BFS on implicit graph|

---

---

## Your Code

```java
class Solution {
    static class Node {
        int i, j, val;
        Node(int i, int j, int val) {
            this.i = i;
            this.j = j;
            this.val = val;
        }
    }

    public int orangesRotting(int[][] arr) {
        Queue<Node> queue = new LinkedList<>();
        int freshCounter = 0;

        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr[0].length; j++) {
                if (arr[i][j] == 2) {
                    Node n = new Node(i, j, arr[i][j]);
                    queue.add(n);
                } else if (arr[i][j] == 1) {
                    freshCounter++;
                }
            }
        }

        if (freshCounter == 0) {
            return 0;
        }

        int time = 0;
        int[][] direction = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int[][] visited = new int[arr.length][arr[0].length];

        while (!queue.isEmpty()) {
            int size = queue.size();
            int counter = 0;
            boolean timePassed = false;

            while (counter < size) {
                Node n = queue.poll();
                for (int i = 0; i < 4; i++) {
                    int[] currDir = direction[i];
                    int currI = n.i + currDir[0];
                    int currJ = n.j + currDir[1];

                    if (currI < arr.length && currI >= 0 && currJ < arr[0].length && currJ >= 0) {
                        if (arr[n.i + currDir[0]][n.j + currDir[1]] == 1) {
                            if (!timePassed) {
                                time++;
                                timePassed = true;
                            }
                            freshCounter--;
                            arr[n.i + currDir[0]][n.j + currDir[1]] = 2;
                            queue.add(new Node(n.i + currDir[0], n.j + currDir[1],
                                    arr[n.i + currDir[0]][n.j + currDir[1]]));
                        }
                    }
                }
                counter++;
            }
        }

        if (freshCounter == 0) {
            return time;
        } else {
            return -1;
        }
    }
}
```

### Notes on Your Code

- ✅ **Multi-source BFS** — correctly enqueues all initial rotten oranges before BFS starts.
- ✅ **Level-by-level processing** — the inner `while (counter < size)` loop correctly isolates each BFS level.
- ✅ **`timePassed` flag** — correctly avoids incrementing `time` in levels where no orange was rotted.
- ✅ **Marks grid cell as `2`** before enqueuing — prevents duplicate enqueuing.
- ⚠️ **`visited` array is declared but never used** — can be safely removed.
- ⚠️ **`Node.val` field is never read** — the value is set but never used since grid cells are checked directly via `arr[i][j]`. The `Node` class can be simplified to just store `(i, j)`, or replaced with `int[]` to reduce boilerplate.
- ⚠️ **`currI`/`currJ` computed twice** — `currI` and `currJ` are computed but then the actual array accesses use `n.i + currDir[0]` again directly. Minor redundancy — using `currI`/`currJ` consistently throughout would be cleaner.