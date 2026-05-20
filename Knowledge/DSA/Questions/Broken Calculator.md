# Broken Calculator

## Tags
- dsa
- greedy
- math
- leetcode

```java
class Solution {
    public int brokenCalc(int startValue, int target) {
        int ops = 0;

        while(target > startValue){
            ops++;

            if(target % 2 == 1) target++;
            else target /= 2;
        }

        return ops + startValue - target;
    }
}
```
