# Unique Paths

## Problem
There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return the number of possible unique paths that the robot can take to reach the bottom-right corner.


## Solution

### Approach (Dynamic Programming)
We use a 2D DP array where each cell represents the number of ways to reach that cell.

- From any cell `(i, j)`, we can come from:
  - Top `(i-1, j)`
  - Left `(i, j-1)`
- So, `dp[i][j] = dp[i-1][j] + dp[i][j-1]`

### Base Cases
- First row = 1 (only right moves)
- First column = 1 (only down moves)

---

### Java Code
```java
public class UniquePaths {
    public static int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];

        // Fill first row
        for (int j = 0; j < n; j++) {
            dp[0][j] = 1;
        }

        // Fill first column
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }

        // Fill rest of grid
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }

        return dp[m-1][n-1];
    }

    public static void main(String[] args) {
        System.out.println(uniquePaths(3, 7)); // Output: 28
    }
}
```

---

### Optimized (1D DP)
```java
public static int uniquePaths(int m, int n) {
    int[] dp = new int[n];

    for (int j = 0; j < n; j++) {
        dp[j] = 1;
    }

    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[j] = dp[j] + dp[j-1];
        }
    }

    return dp[n-1];
}
```

---

### Time & Space Complexity
- Time: O(m × n)
- Space: O(m × n) or O(n) (optimized)


---

## Tags
#DSA #DynamicProgramming #Grid #Combinatorics #Medium

## Topics Covered
- Dynamic Programming (2D DP, 1D Optimization)
- Grid Traversal
- Combinatorics (nCr)
- Recurrence Relation
