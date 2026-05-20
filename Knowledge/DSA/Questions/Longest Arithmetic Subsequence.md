# Longest Arithmetic Subsequence

## Tags
- dsa
- dynamic-programming
- hashmap
- arrays
- leetcode

```java
class Solution {
    public int longestArithSeqLength(int[] nums) {
        int n = nums.length;

        Map<Integer,Integer>[] dp = new HashMap[n];

        for(int i=0;i<n;i++) dp[i]=new HashMap<>();

        int ans = 2;

        for(int i=1;i<n;i++){
            for(int j=0;j<i;j++){
                int diff = nums[i]-nums[j];

                int len = dp[j].getOrDefault(diff,1)+1;
                dp[i].put(diff,len);

                ans=Math.max(ans,len);
            }
        }

        return ans;
    }
}
```
