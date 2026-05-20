# Numbers With Same Consecutive Differences

## Tags
- dsa
- bfs
- recursion
- leetcode

```java
class Solution {
    public int[] numsSameConsecDiff(int n, int k) {
        Queue<Integer> q = new LinkedList<>();

        for(int i=1;i<=9;i++) q.offer(i);

        for(int level=1; level<n; level++){
            int size = q.size();

            while(size-- > 0){
                int num = q.poll();
                int last = num % 10;

                if(last + k < 10)
                    q.offer(num * 10 + last + k);

                if(k != 0 && last - k >= 0)
                    q.offer(num * 10 + last - k);
            }
        }

        return q.stream().mapToInt(i->i).toArray();
    }
}
```
