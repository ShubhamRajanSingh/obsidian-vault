# Find Numbers with Even Number of Digits

```java
class Solution {
    public int findNumbers(int[] nums){
        int ans=0;

        for(int x:nums){
            if(String.valueOf(x).length()%2==0)
                ans++;
        }

        return ans;
    }
}
```