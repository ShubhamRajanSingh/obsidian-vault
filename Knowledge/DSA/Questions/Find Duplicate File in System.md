# Find Duplicate File in System

```java
class Solution {
    public List<List<String>> findDuplicate(String[] paths){
        Map<String,List<String>> map=new HashMap<>();

        for(String path:paths){
            String[] arr=path.split(" ");
            String dir=arr[0];

            for(int i=1;i<arr.length;i++){
                int idx=arr[i].indexOf('(');
                String file=arr[i].substring(0,idx);
                String content=arr[i].substring(idx);

                map.computeIfAbsent(content,x->new ArrayList<>())
                   .add(dir+"/"+file);
            }
        }

        List<List<String>> ans=new ArrayList<>();

        for(List<String> list:map.values()){
            if(list.size()>1) ans.add(list);
        }

        return ans;
    }
}
```