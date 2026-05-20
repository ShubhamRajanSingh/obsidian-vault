# Find Winner on a Tic Tac Toe Game Variant

```java
class Solution {
    public String tictactoe(int[][] moves){
        int[] rows=new int[3],cols=new int[3];
        int diag=0,anti=0,p=1;

        for(int[] m:moves){
            rows[m[0]]+=p;
            cols[m[1]]+=p;

            if(m[0]==m[1]) diag+=p;
            if(m[0]+m[1]==2) anti+=p;

            if(Math.abs(rows[m[0]])==3||Math.abs(cols[m[1]])==3||
               Math.abs(diag)==3||Math.abs(anti)==3)
                return p==1?"A":"B";

            p*=-1;
        }

        return moves.length==9?"Draw":"Pending";
    }
}
```