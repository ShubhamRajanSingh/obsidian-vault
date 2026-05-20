# Network Delay Time

## Tags
- dsa
- graphs
- dijkstra
- heap
- leetcode

```java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        Map<Integer, List<int[]>> graph = new HashMap<>();

        for (int[] t : times) {
            graph.computeIfAbsent(t[0], x -> new ArrayList<>())
                 .add(new int[]{t[1], t[2]});
        }

        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b)->a[1]-b[1]);
        pq.offer(new int[]{k,0});

        boolean[] visited = new boolean[n+1];
        int time = 0;
        int count = 0;

        while(!pq.isEmpty()) {
            int[] cur = pq.poll();
            int node = cur[0], dist = cur[1];

            if(visited[node]) continue;
            visited[node] = true;

            time = dist;
            count++;

            for(int[] nei : graph.getOrDefault(node,new ArrayList<>())) {
                if(!visited[nei[0]]) {
                    pq.offer(new int[]{nei[0], dist + nei[1]});
                }
            }
        }

        return count == n ? time : -1;
    }
}
```
