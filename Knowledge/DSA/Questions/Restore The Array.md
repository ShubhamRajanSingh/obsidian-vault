# Restore The Array

```java
class Solution {
    int mod=1000000007;
    Integer[] memo;

    public int numberOfArrays(String s,int k){
        memo=new Integer[s.length()];
        return dfs(s,k,0);
    }

    int dfs(String s,int k,int idx){
        if(idx==s.length()) return 1;
        if(s.charAt(idx)=='0') return 0;
        if(memo[idx]!=null) return memo[idx];

        long num=0;
        int ans=0;

        for(int i=idx;i<s.length();i++){
            num=num*10+s.charAt(i)-'0';
            if(num>k) break;

            ans=(ans+dfs(s,k,i+1))%mod;
        }

        return memo[idx]=ans;
    }
}
```