# Count Hills and Valleys in an Array

```java
class Solution {
    public int countHillValley(int[] nums){
        int ans=0;

        for(int i=1;i<nums.length-1;i++){
            int l=i-1,r=i+1;

            while(l>=0&&nums[l]==nums[i]) l--;
            while(r<nums.length&&nums[r]==nums[i]) r++;

            if(l>=0&&r<nums.length){
                if(nums[i]>nums[l]&&nums[i]>nums[r]) ans++;
                if(nums[i]<nums[l]&&nums[i]<nums[r]) ans++;
            }
        }

        return ans;
    }
}
```