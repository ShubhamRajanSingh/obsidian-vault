# Rotting Oranges BFS Variant

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        Queue<int[]> q = new LinkedList<>();
        int fresh = 0;

        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j]==2) q.offer(new int[]{i,j});
                else if(grid[i][j]==1) fresh++;
            }
        }

        int time = 0;
        int[][] dirs={{1,0},{-1,0},{0,1},{0,-1}};

        while(!q.isEmpty() && fresh > 0){
            int size=q.size();
            time++;

            while(size-- > 0){
                int[] cell=q.poll();

                for(int[] d:dirs){
                    int nr=cell[0]+d[0];
                    int nc=cell[1]+d[1];

                    if(nr>=0&&nc>=0&&nr<grid.length&&nc<grid[0].length&&grid[nr][nc]==1){
                        grid[nr][nc]=2;
                        fresh--;
                        q.offer(new int[]{nr,nc});
                    }
                }
            }
        }

        return fresh==0?time:-1;
    }
}
```
