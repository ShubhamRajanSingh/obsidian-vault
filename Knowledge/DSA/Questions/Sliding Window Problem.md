# Sliding Window - Maximum Sum Subarray of Size K

## Problem
Given an array of integers and a number k, find the maximum sum of any contiguous subarray of size k.

### Example
Input: arr = [2, 1, 5, 1, 3, 2], k = 3  
Output: 9  
Explanation: Subarray [5, 1, 3] has the maximum sum = 9

---

## Brute Force Approach
Check all subarrays of size k and compute their sum.

Time Complexity: O(n * k)

---

## Optimal Approach (Sliding Window)

### Idea
- Compute sum of first window of size k
- Slide the window by one element at a time:
  - Subtract the element going out
  - Add the element coming in
- Track the maximum sum

### Code (Python)
```python
def max_sum_subarray(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum

    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]
        max_sum = max(max_sum, window_sum)

    return max_sum
```

---

## Key Takeaways
- Sliding window reduces time complexity to O(n)
- Avoid recomputing sums
- Works well for contiguous subarray problems

---

## Variations to Practice
- Smallest subarray with given sum
- Longest substring without repeating characters
- Maximum average subarray