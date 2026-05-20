---
tags:
  - dsa
  - hashmap
  - strings
  - simulation
  - leetcode
leetcode_id: 1487
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Making File Names Unique

## Problem Link
- https://leetcode.com/problems/making-file-names-unique/

## Intuition
Use a hashmap to track the next available suffix for every filename.

## Approach
1. Iterate through names.
2. If unused, store directly.
3. Otherwise keep appending `(k)` until unique.
4. Update next suffix value.

## Java Solution
```java
class Solution {
    public String[] getFolderNames(String[] names) {
        Map<String, Integer> map = new HashMap<>();
        String[] result = new String[names.length];

        for (int i = 0; i < names.length; i++) {
            String name = names[i];

            if (!map.containsKey(name)) {
                result[i] = name;
                map.put(name, 1);
            } else {
                int k = map.get(name);

                while (map.containsKey(name + "(" + k + ")")) {
                    k++;
                }

                String newName = name + "(" + k + ")";
                result[i] = newName;

                map.put(name, k + 1);
                map.put(newName, 1);
            }
        }

        return result;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Hashmap memoization avoids repeated searches.
- Simulation-heavy implementation problem.
