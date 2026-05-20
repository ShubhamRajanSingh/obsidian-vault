# Search Suggestions System

```java
class Solution {
    public List<List<String>> suggestedProducts(String[] products,String search){
        Arrays.sort(products);
        List<List<String>> ans=new ArrayList<>();
        String prefix="";

        for(char ch:search.toCharArray()){
            prefix+=ch;
            List<String> cur=new ArrayList<>();

            for(String p:products){
                if(p.startsWith(prefix)) cur.add(p);
                if(cur.size()==3) break;
            }

            ans.add(cur);
        }

        return ans;
    }
}
```