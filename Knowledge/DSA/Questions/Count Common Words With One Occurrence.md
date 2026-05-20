# Count Common Words With One Occurrence

```java
class Solution {
    public int countWords(String[] words1,String[] words2){
        Map<String,Integer> a=new HashMap<>();
        Map<String,Integer> b=new HashMap<>();

        for(String s:words1)
            a.put(s,a.getOrDefault(s,0)+1);

        for(String s:words2)
            b.put(s,b.getOrDefault(s,0)+1);

        int ans=0;

        for(String s:a.keySet()){
            if(a.get(s)==1&&b.getOrDefault(s,0)==1)
                ans++;
        }

        return ans;
    }
}
```