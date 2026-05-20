# Largest 3 Same Digit Number in String

```java
class Solution {
    public String largestGoodInteger(String num){
        for(char ch='9';ch>='0';ch--){
            String s=""+ch+ch+ch;
            if(num.contains(s)) return s;
        }

        return "";
    }
}
```