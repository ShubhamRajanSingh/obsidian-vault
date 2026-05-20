---
tags:
  - dsa
  - hashmap
  - greedy
  - sorting
  - leetcode
leetcode_id: 954
complexity:
  time: O(n log n)
  space: O(n)
status: solved
language: java
---

# Array of Doubled Pairs

## Problem Link
- https://leetcode.com/problems/array-of-doubled-pairs/

## Intuition
Smaller absolute values should be processed first to avoid pairing conflicts.

## Approach
1. Count frequencies.
2. Sort by absolute value.
3. Try pairing x with 2x.
4. Reduce frequencies accordingly.

## Java Solution
```java
class Solution {
    public boolean canReorderDoubled(int[] arr) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int num : arr) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        Integer[] nums = Arrays.stream(arr)
                               .boxed()
                               .toArray(Integer[]::new);

        Arrays.sort(nums, Comparator.comparingInt(Math::abs));

        for (int num : nums) {
            if (map.get(num) == 0) continue;

            if (map.getOrDefault(num * 2, 0) == 0) {
                return false;
            }

            map.put(num, map.get(num) - 1);
            map.put(num * 2, map.get(num * 2) - 1);
        }

        return true;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(n)

## Key Takeaways
- Absolute value sorting trick.
- Greedy pairing approach.
