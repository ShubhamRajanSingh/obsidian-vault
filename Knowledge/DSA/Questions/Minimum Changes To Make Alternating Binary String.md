# Minimum Changes To Make Alternating Binary String

```java
class Solution {
    public int minOperations(String s){
        int a=0,b=0;

        for(int i=0;i<s.length();i++){
            char expected=(i%2==0)?'0':'1';

            if(s.charAt(i)!=expected) a++;
            else b++;
        }

        return Math.min(a,b);
    }
}
```