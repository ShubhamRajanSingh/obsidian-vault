# Cells with Odd Values in a Matrix

```java
class Solution {
    public int oddCells(int m,int n,int[][] indices){
        int[] rows=new int[m];
        int[] cols=new int[n];

        for(int[] idx:indices){
            rows[idx[0]]^=1;
            cols[idx[1]]^=1;
        }

        int ans=0;

        for(int r:rows)
            for(int c:cols)
                if((r+c)%2==1) ans++;

        return ans;
    }
}
```