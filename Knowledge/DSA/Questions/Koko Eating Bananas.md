# Koko Eating Bananas

## Tags
- dsa
- binary-search
- arrays
- leetcode

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1;
        int right = Arrays.stream(piles).max().getAsInt();

        while(left < right) {
            int mid = left + (right - left) / 2;

            int hours = 0;

            for(int pile : piles) {
                hours += (pile + mid - 1) / mid;
            }

            if(hours <= h) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return left;
    }
}
```
