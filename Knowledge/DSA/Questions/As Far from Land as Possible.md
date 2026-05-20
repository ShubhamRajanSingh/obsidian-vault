# As Far from Land as Possible

## Tags
- dsa
- bfs
- matrix
- graphs
- leetcode

```java
class Solution {
    public int maxDistance(int[][] grid) {
        Queue<int[]> q = new LinkedList<>();
        int n = grid.length;

        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1) q.offer(new int[]{i,j});
            }
        }

        if(q.isEmpty() || q.size()==n*n) return -1;

        int[][] dirs={{1,0},{-1,0},{0,1},{0,-1}};
        int dist=-1;

        while(!q.isEmpty()){
            int size=q.size();
            dist++;

            while(size-- > 0){
                int[] cell=q.poll();

                for(int[] d:dirs){
                    int nr=cell[0]+d[0];
                    int nc=cell[1]+d[1];

                    if(nr>=0&&nc>=0&&nr<n&&nc<n&&grid[nr][nc]==0){
                        grid[nr][nc]=1;
                        q.offer(new int[]{nr,nc});
                    }
                }
            }
        }

        return dist;
    }
}
```
