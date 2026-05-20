# Find Subsequence of Length K With the Largest Sum

```java
class Solution {
    public int[] maxSubsequence(int[] nums,int k){
        int[][] arr=new int[nums.length][2];

        for(int i=0;i<nums.length;i++){
            arr[i][0]=nums[i];
            arr[i][1]=i;
        }

        Arrays.sort(arr,(a,b)->b[0]-a[0]);

        int[][] top=Arrays.copyOf(arr,k);

        Arrays.sort(top,(a,b)->a[1]-b[1]);

        int[] ans=new int[k];

        for(int i=0;i<k;i++) ans[i]=top[i][0];

        return ans;
    }
}
```