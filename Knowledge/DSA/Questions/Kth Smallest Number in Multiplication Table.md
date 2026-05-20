# Kth Smallest Number in Multiplication Table

## Tags
- dsa
- binary-search
- matrix
- leetcode

## Java Solution
```java
class Solution {
    public int findKthNumber(int m, int n, int k) {
        int left = 1;
        int right = m * n;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (count(mid, m, n) >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return left;
    }

    private int count(int x, int m, int n) {
        int total = 0;

        for (int i = 1; i <= m; i++) {
            total += Math.min(x / i, n);
        }

        return total;
    }
}
```
