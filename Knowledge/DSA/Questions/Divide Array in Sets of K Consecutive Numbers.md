---
tags:
  - dsa
  - hashmap
  - greedy
  - sorting
  - leetcode
leetcode_id: 1296
complexity:
  time: O(n log n)
  space: O(n)
status: solved
language: java
---

# Divide Array in Sets of K Consecutive Numbers

## Problem Link
- https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/

## Intuition
Always start building groups from the smallest available number.
Greedy works because smaller numbers have fewer placement options.

## Approach
1. Store frequencies using TreeMap.
2. Repeatedly take smallest key.
3. Try forming sequence of length k.
4. If any required number is unavailable, return false.

## Java Solution
```java
class Solution {
    public boolean isPossibleDivide(int[] nums, int k) {
        if (nums.length % k != 0) {
            return false;
        }

        TreeMap<Integer, Integer> map = new TreeMap<>();

        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        while (!map.isEmpty()) {
            int first = map.firstKey();

            for (int i = 0; i < k; i++) {
                int current = first + i;

                if (!map.containsKey(current)) {
                    return false;
                }

                map.put(current, map.get(current) - 1);

                if (map.get(current) == 0) {
                    map.remove(current);
                }
            }
        }

        return true;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(n)

## Key Takeaways
- TreeMap helps maintain sorted order.
- Greedy interval building pattern.
