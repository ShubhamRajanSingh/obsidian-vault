# Open the Lock

## Tags
- dsa
- bfs
- graphs
- strings
- leetcode

```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> dead = new HashSet<>(Arrays.asList(deadends));

        if(dead.contains("0000")) return -1;

        Queue<String> q = new LinkedList<>();
        Set<String> visited = new HashSet<>();

        q.offer("0000");
        visited.add("0000");

        int steps = 0;

        while(!q.isEmpty()) {
            int size = q.size();

            for(int i = 0; i < size; i++) {
                String cur = q.poll();

                if(cur.equals(target)) return steps;

                for(String next : neighbors(cur)) {
                    if(!dead.contains(next) && visited.add(next)) {
                        q.offer(next);
                    }
                }
            }

            steps++;
        }

        return -1;
    }

    private List<String> neighbors(String s) {
        List<String> list = new ArrayList<>();

        char[] arr = s.toCharArray();

        for(int i = 0; i < 4; i++) {
            char ch = arr[i];

            arr[i] = ch == '9' ? '0' : (char)(ch + 1);
            list.add(new String(arr));

            arr[i] = ch == '0' ? '9' : (char)(ch - 1);
            list.add(new String(arr));

            arr[i] = ch;
        }

        return list;
    }
}
```
