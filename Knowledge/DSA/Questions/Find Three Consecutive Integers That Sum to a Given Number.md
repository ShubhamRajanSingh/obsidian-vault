# Find Three Consecutive Integers That Sum to a Given Number

```java
class Solution {
    public long[] sumOfThree(long num){
        if(num%3!=0) return new long[]{};

        long x=num/3;

        return new long[]{x-1,x,x+1};
    }
}
```