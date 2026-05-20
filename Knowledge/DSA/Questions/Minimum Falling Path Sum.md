# Minimum Falling Path Sum

## Tags
- dsa
- dynamic-programming
- matrix
- leetcode

```java
class Solution {
    public int minFallingPathSum(int[][] matrix) {
        int n = matrix.length;

        for(int i = 1; i < n; i++) {
            for(int j = 0; j < n; j++) {
                int best = matrix[i-1][j];

                if(j > 0) best = Math.min(best, matrix[i-1][j-1]);
                if(j + 1 < n) best = Math.min(best, matrix[i-1][j+1]);

                matrix[i][j] += best;
            }
        }

        int ans = Integer.MAX_VALUE;

        for(int x : matrix[n-1]) ans = Math.min(ans, x);

        return ans;
    }
}
```
