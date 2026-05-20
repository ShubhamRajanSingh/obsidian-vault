# Kids With the Greatest Number of Candies

```java
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies,int extra){
        int max=0;

        for(int x:candies) max=Math.max(max,x);

        List<Boolean> ans=new ArrayList<>();

        for(int x:candies)
            ans.add(x+extra>=max);

        return ans;
    }
}
```