# Path With Maximum Gold

```java
class Solution {
    int ans=0;

    public int getMaximumGold(int[][] grid){
        for(int i=0;i<grid.length;i++)
            for(int j=0;j<grid[0].length;j++)
                dfs(grid,i,j,0);

        return ans;
    }

    void dfs(int[][] g,int r,int c,int sum){
        if(r<0||c<0||r==g.length||c==g[0].length||g[r][c]==0)
            return;

        int val=g[r][c];
        g[r][c]=0;

        sum+=val;
        ans=Math.max(ans,sum);

        dfs(g,r+1,c,sum);
        dfs(g,r-1,c,sum);
        dfs(g,r,c+1,sum);
        dfs(g,r,c-1,sum);

        g[r][c]=val;
    }
}
```