# Unique Paths III

## Tags
- dsa
- backtracking
- dfs
- grid
- leetcode

```java
class Solution {
    int paths = 0;
    int empty = 1;

    public int uniquePathsIII(int[][] grid) {
        int sr = 0, sc = 0;

        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j] == 0) empty++;
                else if(grid[i][j] == 1){
                    sr = i;
                    sc = j;
                }
            }
        }

        dfs(grid,sr,sc,0);
        return paths;
    }

    private void dfs(int[][] g,int r,int c,int count){
        if(r<0||c<0||r==g.length||c==g[0].length||g[r][c]==-1) return;

        if(g[r][c]==2){
            if(count==empty) paths++;
            return;
        }

        int temp=g[r][c];
        g[r][c]=-1;

        dfs(g,r+1,c,count+1);
        dfs(g,r-1,c,count+1);
        dfs(g,r,c+1,count+1);
        dfs(g,r,c-1,count+1);

        g[r][c]=temp;
    }
}
```
