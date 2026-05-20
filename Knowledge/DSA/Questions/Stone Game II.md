# Stone Game II

```java
class Solution {
    int[][] memo;
    int[] suffix;

    public int stoneGameII(int[] piles){
        int n=piles.length;
        memo=new int[n][n+1];
        suffix=new int[n+1];

        for(int i=n-1;i>=0;i--)
            suffix[i]=suffix[i+1]+piles[i];

        return dfs(0,1,piles);
    }

    private int dfs(int i,int m,int[] piles){
        if(i>=piles.length) return 0;
        if(2*m>=piles.length-i) return suffix[i];

        if(memo[i][m]!=0) return memo[i][m];

        int best=0;

        for(int x=1;x<=2*m;x++){
            best=Math.max(best,
                suffix[i]-dfs(i+x,Math.max(m,x),piles));
        }

        return memo[i][m]=best;
    }
}
```