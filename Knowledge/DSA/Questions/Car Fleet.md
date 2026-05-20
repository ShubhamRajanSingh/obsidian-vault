# Car Fleet

## Tags
- dsa
- greedy
- sorting
- stack
- leetcode

```java
class Solution {
    public int carFleet(int target,
                        int[] position,
                        int[] speed) {

        int n = position.length;

        int[][] cars = new int[n][2];

        for(int i = 0; i < n; i++) {
            cars[i] = new int[]{position[i], speed[i]};
        }

        Arrays.sort(cars, (a,b) -> b[0] - a[0]);

        int fleets = 0;
        double time = 0;

        for(int[] car : cars) {
            double current = (double)(target - car[0]) / car[1];

            if(current > time) {
                fleets++;
                time = current;
            }
        }

        return fleets;
    }
}
```
