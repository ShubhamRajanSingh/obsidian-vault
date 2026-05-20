# Richest Customer Wealth

```java
class Solution {
    public int maximumWealth(int[][] accounts){
        int ans=0;

        for(int[] row:accounts){
            int sum=0;
            for(int x:row) sum+=x;
            ans=Math.max(ans,sum);
        }

        return ans;
    }
}
```