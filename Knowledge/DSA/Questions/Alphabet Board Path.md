# Alphabet Board Path

```java
class Solution {
    public String alphabetBoardPath(String target){
        int r=0,c=0;
        StringBuilder sb=new StringBuilder();

        for(char ch:target.toCharArray()){
            int nr=(ch-'a')/5;
            int nc=(ch-'a')%5;

            while(r>nr){sb.append('U');r--;}
            while(c>nc){sb.append('L');c--;}
            while(r<nr){sb.append('D');r++;}
            while(c<nc){sb.append('R');c++;}

            sb.append('!');
        }

        return sb.toString();
    }
}
```