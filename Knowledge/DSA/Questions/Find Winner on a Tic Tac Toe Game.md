# Find Winner on a Tic Tac Toe Game

```java
class Solution {
    public String tictactoe(int[][] moves){
        int[] rows=new int[3],cols=new int[3];
        int diag=0,anti=0;

        int player=1;

        for(int[] m:moves){
            rows[m[0]]+=player;
            cols[m[1]]+=player;

            if(m[0]==m[1]) diag+=player;
            if(m[0]+m[1]==2) anti+=player;

            if(Math.abs(rows[m[0]])==3||Math.abs(cols[m[1]])==3||
               Math.abs(diag)==3||Math.abs(anti)==3)
                return player==1?"A":"B";

            player*=-1;
        }

        return moves.length==9?"Draw":"Pending";
    }
}
```