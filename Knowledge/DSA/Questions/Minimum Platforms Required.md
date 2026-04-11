# Minimum Platforms Required

## Tags
#DSA #Greedy #Sorting #InterviewPrep

## Approach (Greedy + Sorting)
- Sort arrival and departure
- Use two pointers

```java
import java.util.*;

class Solution {
    static int findPlatform(int arr[], int dep[]) {
        Arrays.sort(arr);
        Arrays.sort(dep);

        int i = 0, j = 0, platforms = 0, max = 0;

        while (i < arr.length && j < dep.length) {
            if (arr[i] <= dep[j]) {
                platforms++;
                max = Math.max(max, platforms);
                i++;
            } else {
                platforms--;
                j++;
            }
        }

        return max;
    }
}
```