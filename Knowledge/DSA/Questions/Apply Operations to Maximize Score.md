---
tags:
  - dsa
  - greedy
  - monotonic-stack
  - math
  - leetcode
leetcode_id: 2818
complexity:
  time: O(n log n)
  space: O(n)
status: solved
language: java
---

# Apply Operations to Maximize Score

## Problem Link
- https://leetcode.com/problems/apply-operations-to-maximize-score/

## Intuition
We want to maximize score contribution from large values.
Monotonic stack helps determine how many subarrays each element can dominate.

## Approach
1. Compute prime scores.
2. Use monotonic stack to find contribution range.
3. Sort numbers descending.
4. Greedily use largest values first.
5. Apply fast exponentiation.

## Java Solution
```java
class Solution {
    static final int MOD = 1_000_000_007;

    public int maximumScore(List<Integer> nums, int k) {
        int n = nums.size();
        int[] primeScore = new int[n];

        for (int i = 0; i < n; i++) {
            primeScore[i] = countPrimeFactors(nums.get(i));
        }

        long[] left = new long[n];
        long[] right = new long[n];

        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() &&
                    primeScore[stack.peek()] < primeScore[i]) {
                stack.pop();
            }

            left[i] = stack.isEmpty() ? i + 1 : i - stack.peek();
            stack.push(i);
        }

        stack.clear();

        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() &&
                    primeScore[stack.peek()] <= primeScore[i]) {
                stack.pop();
            }

            right[i] = stack.isEmpty() ? n - i : stack.peek() - i;
            stack.push(i);
        }

        List<int[]> arr = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            arr.add(new int[]{nums.get(i), i});
        }

        arr.sort((a, b) -> b[0] - a[0]);

        long answer = 1;

        for (int[] item : arr) {
            int value = item[0];
            int idx = item[1];

            long count = left[idx] * right[idx];
            long use = Math.min(count, k);

            answer = (answer * modPow(value, use)) % MOD;
            k -= use;

            if (k == 0) break;
        }

        return (int) answer;
    }

    private int countPrimeFactors(int num) {
        int count = 0;

        for (int i = 2; i * i <= num; i++) {
            if (num % i == 0) {
                count++;
                while (num % i == 0) {
                    num /= i;
                }
            }
        }

        if (num > 1) count++;

        return count;
    }

    private long modPow(long base, long exp) {
        long result = 1;

        while (exp > 0) {
            if ((exp & 1) == 1) {
                result = (result * base) % MOD;
            }

            base = (base * base) % MOD;
            exp >>= 1;
        }

        return result;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(n)

## Key Takeaways
- Monotonic stack contribution technique.
- Greedy selection with modular exponentiation.
