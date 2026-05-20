---
tags:
  - dsa
  - heap
  - sorting
  - simulation
  - greedy
  - leetcode
leetcode_id: 1834
complexity:
  time: O(n log n)
  space: O(n)
status: solved
language: java
---

# Single-Threaded CPU

## Problem Link
- https://leetcode.com/problems/single-threaded-cpu/

## Intuition
Tasks become available over time.
Priority queue selects shortest available task.

## Approach
1. Sort tasks by enqueue time.
2. Use min-heap ordered by:
   - processing time
   - index
3. Simulate CPU execution.
4. Add newly available tasks dynamically.

## Java Solution
```java
class Solution {
    public int[] getOrder(int[][] tasks) {
        int n = tasks.length;

        int[][] indexed = new int[n][3];

        for (int i = 0; i < n; i++) {
            indexed[i] = new int[]{tasks[i][0], tasks[i][1], i};
        }

        Arrays.sort(indexed, (a, b) -> a[0] - b[0]);

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
            if (a[1] != b[1]) {
                return a[1] - b[1];
            }

            return a[2] - b[2];
        });

        int[] answer = new int[n];

        long time = 0;
        int i = 0;
        int idx = 0;

        while (i < n || !pq.isEmpty()) {
            if (pq.isEmpty()) {
                time = Math.max(time, indexed[i][0]);
            }

            while (i < n && indexed[i][0] <= time) {
                pq.offer(indexed[i++]);
            }

            int[] task = pq.poll();

            time += task[1];
            answer[idx++] = task[2];
        }

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log n)
- Space Complexity: O(n)

## Key Takeaways
- Scheduling simulation.
- Heap + sorting integration.
