# How Many Numbers Are Smaller Than the Current Number

```java
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums){
        int[] sorted=nums.clone();
        Arrays.sort(sorted);

        Map<Integer,Integer> map=new HashMap<>();

        for(int i=0;i<sorted.length;i++)
            map.putIfAbsent(sorted[i],i);

        int[] ans=new int[nums.length];

        for(int i=0;i<nums.length;i++)
            ans[i]=map.get(nums[i]);

        return ans;
    }
}
```