---
tags:
  - dsa
  - heap
  - hashmap
  - sorting
  - strings
  - leetcode
leetcode_id: 692
complexity:
  time: O(n log k)
  space: O(n)
status: solved
language: java
---

# Top K Frequent Words

## Problem Link
- https://leetcode.com/problems/top-k-frequent-words/

## Intuition
Use hashmap for frequencies and heap for maintaining top k words.

## Approach
1. Count word frequencies.
2. Maintain min-heap ordered by:
   - frequency
   - reverse lexicographical order
3. Extract final result in reverse order.

## Java Solution
```java
class Solution {
    public List<String> topKFrequent(String[] words,
                                     int k) {

        Map<String, Integer> freq = new HashMap<>();

        for (String word : words) {
            freq.put(word,
                freq.getOrDefault(word, 0) + 1);
        }

        PriorityQueue<String> pq = new PriorityQueue<>((a, b) -> {
            if (!freq.get(a).equals(freq.get(b))) {
                return freq.get(a) - freq.get(b);
            }

            return b.compareTo(a);
        });

        for (String word : freq.keySet()) {
            pq.offer(word);

            if (pq.size() > k) {
                pq.poll();
            }
        }

        List<String> answer = new ArrayList<>();

        while (!pq.isEmpty()) {
            answer.add(pq.poll());
        }

        Collections.reverse(answer);

        return answer;
    }
}
```

## Complexity Analysis
- Time Complexity: O(n log k)
- Space Complexity: O(n)

## Key Takeaways
- Heap ordering customization.
- Frequency ranking problems.
