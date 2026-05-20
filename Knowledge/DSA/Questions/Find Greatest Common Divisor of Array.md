# Find Greatest Common Divisor of Array

```java
class Solution {
    public int findGCD(int[] nums){
        int min=Integer.MAX_VALUE,max=Integer.MIN_VALUE;

        for(int x:nums){
            min=Math.min(min,x);
            max=Math.max(max,x);
        }

        return gcd(min,max);
    }

    int gcd(int a,int b){
        return b==0?a:gcd(b,a%b);
    }
}
```