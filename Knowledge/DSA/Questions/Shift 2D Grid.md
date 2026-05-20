# Shift 2D Grid

```java
class Solution {
    public List<List<Integer>> shiftGrid(int[][] grid,int k){
        int m=grid.length,n=grid[0].length;
        int total=m*n;

        List<List<Integer>> ans=new ArrayList<>();

        for(int i=0;i<m;i++) ans.add(new ArrayList<>());

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                int idx=(i*n+j+k)%total;
                int r=idx/n,c=idx%n;

                while(ans.get(r).size()<c)
                    ans.get(r).add(0);

                ans.get(r).add(grid[i][j]);
            }
        }

        return ans;
    }
}
```