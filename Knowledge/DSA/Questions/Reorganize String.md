# Reorganize String

## Tags
- dsa
- heap
- greedy
- strings
- leetcode

```java
class Solution {
    public String reorganizeString(String s) {
        int[] freq = new int[26];

        for (char ch : s.toCharArray()) {
            freq[ch - 'a']++;
        }

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[1] - a[1]);

        for (int i = 0; i < 26; i++) {
            if (freq[i] > 0) {
                pq.offer(new int[]{i, freq[i]});
            }
        }

        StringBuilder sb = new StringBuilder();

        while (pq.size() >= 2) {
            int[] a = pq.poll();
            int[] b = pq.poll();

            sb.append((char)(a[0] + 'a'));
            sb.append((char)(b[0] + 'a'));

            if (--a[1] > 0) pq.offer(a);
            if (--b[1] > 0) pq.offer(b);
        }

        if (!pq.isEmpty()) {
            if (pq.peek()[1] > 1) return "";
            sb.append((char)(pq.poll()[0] + 'a'));
        }

        return sb.toString();
    }
}
```
