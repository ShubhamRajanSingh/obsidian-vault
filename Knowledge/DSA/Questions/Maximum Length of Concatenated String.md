# Maximum Length of Concatenated String

```java
class Solution {
    int ans=0;

    public int maxLength(List<String> arr){
        dfs(arr,0,"");
        return ans;
    }

    void dfs(List<String> arr,int idx,String cur){
        if(!valid(cur)) return;

        ans=Math.max(ans,cur.length());

        for(int i=idx;i<arr.size();i++)
            dfs(arr,i+1,cur+arr.get(i));
    }

    boolean valid(String s){
        int[] f=new int[26];

        for(char ch:s.toCharArray()){
            if(++f[ch-'a']>1) return false;
        }

        return true;
    }
}
```