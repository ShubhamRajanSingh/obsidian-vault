# N-Queens Problem — Complete Explanation

## Problem Statement

Place `n` queens on an `n×n` chessboard such that **no two queens attack each other**.  
Queens attack along rows, columns, and **both diagonals**.

---

## The Core Challenge — Tracking Diagonals

The trickiest part of N-Queens is efficiently checking whether a diagonal is already occupied.  
We use **two arrays** to track both types of diagonals:

|Array|Diagonal Type|Formula (index)|
|---|---|---|
|`row[]`|Column conflict|`col`|
|`leftUpper[]`|Top-left → Bottom-right (↘)|`n - 1 + col - row`|
|`rightUpper[]`|Top-right → Bottom-left (↙)|`row + col`|

---

## 📐 Visual: The Board (n = 4)

```
        col →   0    1    2    3
row
 ↓   0  [ ]  [ ]  [ ]  [ ]
     1  [ ]  [ ]  [ ]  [ ]
     2  [ ]  [ ]  [ ]  [ ]
     3  [ ]  [ ]  [ ]  [ ]
```

We place queens **row by row** (top to bottom).  
For each row `k`, we try every column `i` and check 3 conditions:

1. Column `i` not already attacked → `row[i] == 0`
2. Left-upper diagonal not attacked → `leftUpper[n-1+i-k] == 0`
3. Right-upper diagonal not attacked → `rightUpper[k+i] == 0`

---

## 🔴 Right-Upper Diagonal (`↗` / `↙`)

### What cells share the same right diagonal?

Cells on the same **top-right → bottom-left** diagonal have the **same sum** of `(row + col)`.

```
        col →   0    1    2    3
row
 ↓   0  [0]  [1]  [2]  [3]
     1  [1]  [2]  [3]  [4]
     2  [2]  [3]  [4]  [5]
     3  [3]  [4]  [5]  [6]
```

> Each number = `row + col`

### Formula

```
rightUpper_index = row + col
```

**Range:** `0` to `2n - 2` → Array size needed: `2n - 1`

### Visual: Same diagonal (sum = 3)

```
        col →   0    1    2    3
row
 ↓   0  [ ]  [ ]  [ ]  [*]   ← 0+3=3
     1  [ ]  [ ]  [*]  [ ]   ← 1+2=3
     2  [ ]  [*]  [ ]  [ ]   ← 2+1=3
     3  [*]  [ ]  [ ]  [ ]   ← 3+0=3
```

> All `[*]` cells share `rightUpper[3]` — placing a queen on any one blocks the rest.

---

## 🔵 Left-Upper Diagonal (`↖` / `↘`)

### What cells share the same left diagonal?

Cells on the same **top-left → bottom-right** diagonal have the **same difference** of `(col - row)`.

```
        col →   0    1    2    3
row
 ↓   0  [0]  [1]  [2]  [3]
     1  [-1] [0]  [1]  [2]
     2  [-2] [-1] [0]  [1]
     3  [-3] [-2] [-1] [0]
```

> Each number = `col - row`

### Problem: Negative indices!

`col - row` ranges from `-(n-1)` to `+(n-1)`.  
We can't use negative array indices, so we **shift by `(n-1)`**:

```
leftUpper_index = (n - 1) + col - row
```

**Range:** `0` to `2n - 2` → Array size needed: `2n - 1`

### Visual: Same diagonal (col - row = 1, shifted index = n-1+1 = 4 for n=4)

```
        col →   0    1    2    3
row
 ↓   0  [ ]  [*]  [ ]  [ ]   ← col-row = 1
     1  [ ]  [ ]  [*]  [ ]   ← col-row = 1
     2  [ ]  [ ]  [ ]  [*]   ← col-row = 1
     3  [ ]  [ ]  [ ]  [ ]   ← (out of bounds)
```

> All `[*]` cells share `leftUpper[4]` — placing a queen on any one blocks the rest.

---

## 🗂️ Combined Diagonal Index Map (n = 4)

```
        col →     0       1       2       3
row
 ↓   0  R=0,L=3  R=1,L=4  R=2,L=5  R=3,L=6
     1  R=1,L=2  R=2,L=3  R=3,L=4  R=4,L=5
     2  R=2,L=1  R=3,L=2  R=4,L=3  R=5,L=4
     3  R=3,L=0  R=4,L=1  R=5,L=2  R=6,L=3
```

> `R = row + col` (rightUpper index)  
> `L = (n-1) + col - row` (leftUpper index)

---

## 📊 Formula Summary

|Conflict Type|Index Formula|Array Size|Notes|
|---|---|---|---|
|Column|`col`|`n`|Direct index|
|Right diagonal (↙↗)|`row + col`|`2n - 1`|Sum is constant on diagonal|
|Left diagonal (↘↖)|`(n-1) + col - row`|`2n - 1`|Difference shifted to avoid negatives|

---

## ✅ Validity Check (3 Conditions)

To place a queen at `(k, i)` (row `k`, column `i`):

```java
if (row[i] != 1                          // Column i is free
 && leftUpper[n - 1 + i - k] != 1       // Left diagonal is free
 && rightUpper[k + i] != 1)             // Right diagonal is free
```

---

## 🔁 Backtracking Logic

```
Place queen at (k, i)
    ↓ Mark row, leftUpper, rightUpper
    ↓ Recurse for row k+1
    ↓ Unmark (backtrack)
```

```java
m[k][i] = 'Q';
row[i] = 1;
leftUpper[n - 1 + i - k] = 1;
rightUpper[k + i] = 1;

helper(k + 1, row, leftUpper, rightUpper, m, res);

// Backtrack
m[k][i] = '.';
row[i] = 0;
leftUpper[n - 1 + i - k] = 0;
rightUpper[k + i] = 0;
```

---

## 🧩 Full Annotated Code

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> res = new ArrayList<>();
        int[] row = new int[n];            // tracks occupied columns
        int[] leftupperd = new int[2*n+1]; // left diagonals (↘), index = n-1+col-row
        int[] rightupperd = new int[2*n+1];// right diagonals (↙), index = row+col
        char[][] m = new char[n][n];
        for (int i = 0; i < n; i++) {
            Arrays.fill(m[i], '.');        // initialize board with empty cells
        }
        helper(0, row, leftupperd, rightupperd, m, res);
        return res;
    }

    public static void helper(int k, int[] row, int[] leftUpper, int[] rightUpper,
                               char[][] m, List<List<String>> res) {
        if (k == m.length) {               // All rows filled — valid solution!
            List<String> currList = new ArrayList<>();
            for (int i = 0; i < m.length; i++) {
                currList.add(new String(m[i]));
            }
            res.add(currList);
            return;
        }

        for (int i = 0; i < m.length; i++) {
            // Check: column + left diagonal + right diagonal all free
            if (row[i] != 1
             && leftUpper[m.length - 1 + i - k] != 1
             && rightUpper[k + i] != 1) {

                m[k][i] = 'Q';
                row[i] = 1;
                leftUpper[m.length - 1 + i - k] = 1;
                rightUpper[k + i] = 1;

                helper(k + 1, row, leftUpper, rightUpper, m, res); // recurse

                // Backtrack
                m[k][i] = '.';
                row[i] = 0;
                leftUpper[m.length - 1 + i - k] = 0;
                rightUpper[k + i] = 0;
            }
        }
    }
}
```

---

## 🧪 Trace: n = 4, First Valid Solution

```
Step 1: Place Q at (0,1)
. Q . .
. . . .
. . . .
. . . .

Step 2: Place Q at (1,3)
. Q . .
. . . Q
. . . .
. . . .

Step 3: Place Q at (2,0)
. Q . .
. . . Q
Q . . .
. . . .

Step 4: Place Q at (3,2) → Valid!
. Q . .
. . . Q
Q . . .
. . Q .
```

---

## ⏱️ Complexity

|||
|---|---|
|**Time**|O(n!) — at most n choices for row 0, n-1 for row 1, etc.|
|**Space**|O(n) for tracking arrays + O(n²) for board|

---

## 🔑 Key Insight

The trick that makes this O(1) conflict checking (instead of scanning the board):

> Instead of walking the diagonal to see if it's occupied, we **hash** each diagonal to a unique index. A queen at `(r, c)` claims exactly one index in each of the three arrays — making mark/unmark and check all constant time.
