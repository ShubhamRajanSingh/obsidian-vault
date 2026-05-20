# Maximum Ice Cream Bars

```java
class Solution {
    public int maxIceCream(int[] costs,int coins){
        Arrays.sort(costs);

        int ans=0;

        for(int x:costs){
            if(coins<x) break;
            coins-=x;
            ans++;
        }

        return ans;
    }
}
```