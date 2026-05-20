---
tags:
  - dsa
  - graph
  - dijkstra
  - dynamic-programming
  - medium
  - leetcode
  - striver
aliases:
  - Number of Ways to Arrive at Destination
  - LeetCode 1976
difficulty: Medium
topic: Graph / Dijkstra / Counting Paths
pattern: Dijkstra + count shortest paths
leetcode_id: 1976
date: 2026-05-19
---

# Number of Ways to Arrive at Destination

## 📌 Problem Statement

> There are `n` intersections. Given roads as `[u, v, time]`, find the number of ways to travel from node `0` to node `n-1` in the shortest time. Return the answer modulo 10^9 + 7.

**LeetCode #1976** | Difficulty: 🟡 Medium | Striver SDE Sheet ✅

---

## ✅ Java Solution

```java
class Solution {
    public int countPaths(int n, int[][] roads) {
        long MOD = 1_000_000_007L;
        List<long[]>[] adj = new List[n];
        for (int i = 0; i < n; i++) adj[i] = new ArrayList<>();
        for (int[] r : roads) { adj[r[0]].add(new long[]{r[1], r[2]}); adj[r[1]].add(new long[]{r[0], r[2]}); }

        long[] dist = new long[n]; long[] ways = new long[n];
        Arrays.fill(dist, Long.MAX_VALUE);
        dist[0] = 0; ways[0] = 1;

        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> Long.compare(a[1], b[1]));
        pq.offer(new long[]{0, 0});

        while (!pq.isEmpty()) {
            long[] curr = pq.poll();
            long node = curr[0], d = curr[1];
            if (d > dist[(int)node]) continue;
            for (long[] nei : adj[(int)node]) {
                long nd = d + nei[1];
                if (nd < dist[(int)nei[0]]) { dist[(int)nei[0]] = nd; ways[(int)nei[0]] = ways[(int)node]; pq.offer(new long[]{nei[0], nd}); }
                else if (nd == dist[(int)nei[0]]) ways[(int)nei[0]] = (ways[(int)nei[0]] + ways[(int)node]) % MOD;
            }
        }

        return (int) ways[n - 1];
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| Time | O((V + E) log V) |
| Space | O(V + E) |
