# Find Resultant Array After Removing Anagrams

```java
class Solution {
    public List<String> removeAnagrams(String[] words){
        List<String> ans=new ArrayList<>();

        ans.add(words[0]);

        for(int i=1;i<words.length;i++){
            if(!same(words[i],words[i-1]))
                ans.add(words[i]);
        }

        return ans;
    }

    boolean same(String a,String b){
        char[] x=a.toCharArray();
        char[] y=b.toCharArray();

        Arrays.sort(x);
        Arrays.sort(y);

        return Arrays.equals(x,y);
    }
}
```