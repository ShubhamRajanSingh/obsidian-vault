# Keys and Rooms

## Tags
- dsa
- dfs
- bfs
- graphs
- leetcode

```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        boolean[] visited = new boolean[rooms.size()];

        dfs(rooms, 0, visited);

        for(boolean room : visited) {
            if(!room) return false;
        }

        return true;
    }

    private void dfs(List<List<Integer>> rooms,
                     int room,
                     boolean[] visited) {

        if(visited[room]) return;

        visited[room] = true;

        for(int key : rooms.get(room)) {
            dfs(rooms, key, visited);
        }
    }
}
```
