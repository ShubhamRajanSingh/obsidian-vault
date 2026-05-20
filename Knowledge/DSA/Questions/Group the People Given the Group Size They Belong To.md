# Group the People Given the Group Size They Belong To

```java
class Solution {
    public List<List<Integer>> groupThePeople(int[] groupSizes){
        Map<Integer,List<Integer>> map=new HashMap<>();
        List<List<Integer>> ans=new ArrayList<>();

        for(int i=0;i<groupSizes.length;i++){
            int g=groupSizes[i];

            map.computeIfAbsent(g,x->new ArrayList<>()).add(i);

            if(map.get(g).size()==g){
                ans.add(new ArrayList<>(map.get(g)));
                map.get(g).clear();
            }
        }

        return ans;
    }
}
```