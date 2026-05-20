# Remove Digit From Number to Maximize Result

```java
class Solution {
    public String removeDigit(String number,char digit){
        String ans="";

        for(int i=0;i<number.length();i++){
            if(number.charAt(i)==digit){
                String cur=number.substring(0,i)+number.substring(i+1);

                if(cur.compareTo(ans)>0) ans=cur;
            }
        }

        return ans;
    }
}
```