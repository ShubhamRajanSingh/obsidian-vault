# Find if Path Exists in Graph

```java
class Solution {
    public boolean validPath(int n,int[][] edges,int source,int dest){
        List<List<Integer>> graph=new ArrayList<>();

        for(int i=0;i<n;i++) graph.add(new ArrayList<>());

        for(int[] e:edges){
            graph.get(e[0]).add(e[1]);
            graph.get(e[1]).add(e[0]);
        }

        Queue<Integer> q=new LinkedList<>();
        boolean[] vis=new boolean[n];

        q.offer(source);
        vis[source]=true;

        while(!q.isEmpty()){
            int node=q.poll();

            if(node==dest) return true;

            for(int next:graph.get(node)){
                if(!vis[next]){
                    vis[next]=true;
                    q.offer(next);
                }
            }
        }

        return false;
    }
}
```