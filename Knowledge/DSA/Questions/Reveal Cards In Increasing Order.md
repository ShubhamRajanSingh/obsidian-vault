# Reveal Cards In Increasing Order

## Tags
- dsa
- queue
- sorting
- leetcode

```java
class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        Arrays.sort(deck);
        Queue<Integer> q = new LinkedList<>();
        int n = deck.length;

        for(int i=0;i<n;i++) q.offer(i);

        int[] ans = new int[n];

        for(int card : deck){
            ans[q.poll()] = card;
            if(!q.isEmpty()) q.offer(q.poll());
        }

        return ans;
    }
}
```
