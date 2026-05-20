# Count Negative Numbers in a Sorted Matrix

```java
class Solution {
    public int countNegatives(int[][] grid){
        int r=grid.length-1,c=0;
        int ans=0;

        while(r>=0&&c<grid[0].length){
            if(grid[r][c]<0){
                ans+=grid[0].length-c;
                r--;
            }else c++;
        }

        return ans;
    }
}
```