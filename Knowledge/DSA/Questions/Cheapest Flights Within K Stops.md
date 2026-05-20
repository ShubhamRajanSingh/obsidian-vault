# Cheapest Flights Within K Stops

## Tags
- dsa
- graphs
- bfs
- shortest-path
- leetcode

```java
class Solution {
    public int findCheapestPrice(int n,
                                 int[][] flights,
                                 int src,
                                 int dst,
                                 int k) {

        Map<Integer, List<int[]>> graph = new HashMap<>();

        for (int[] f : flights) {
            graph.computeIfAbsent(f[0], x -> new ArrayList<>())
                 .add(new int[]{f[1], f[2]});
        }

        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[]{src, 0, 0});

        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);

        while (!q.isEmpty()) {
            int[] cur = q.poll();
            int node = cur[0], cost = cur[1], stops = cur[2];

            if (stops > k) continue;

            for (int[] nei : graph.getOrDefault(node, new ArrayList<>())) {
                int next = nei[0], price = nei[1];

                if (cost + price < dist[next]) {
                    dist[next] = cost + price;
                    q.offer(new int[]{next, dist[next], stops + 1});
                }
            }
        }

        return dist[dst] == Integer.MAX_VALUE ? -1 : dist[dst];
    }
}
```
