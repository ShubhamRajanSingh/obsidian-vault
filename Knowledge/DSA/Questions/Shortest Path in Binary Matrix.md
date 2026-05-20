# Shortest Path in Binary Matrix

## Tags
- dsa
- bfs
- matrix
- graphs
- leetcode

```java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        if(grid[0][0]==1) return -1;

        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[]{0,0,1});

        grid[0][0]=1;

        int[][] dirs={{1,0},{-1,0},{0,1},{0,-1},{1,1},{1,-1},{-1,1},{-1,-1}};

        while(!q.isEmpty()){
            int[] cur=q.poll();

            if(cur[0]==grid.length-1 && cur[1]==grid[0].length-1)
                return cur[2];

            for(int[] d:dirs){
                int nr=cur[0]+d[0];
                int nc=cur[1]+d[1];

                if(nr>=0&&nc>=0&&nr<grid.length&&nc<grid[0].length&&grid[nr][nc]==0){
                    grid[nr][nc]=1;
                    q.offer(new int[]{nr,nc,cur[2]+1});
                }
            }
        }

        return -1;
    }
}
```
