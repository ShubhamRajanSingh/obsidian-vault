# Find First Palindromic String in the Array

```java
class Solution {
    public String firstPalindrome(String[] words){
        for(String s:words){
            if(pal(s)) return s;
        }

        return "";
    }

    boolean pal(String s){
        int l=0,r=s.length()-1;

        while(l<r)
            if(s.charAt(l++)!=s.charAt(r--))
                return false;

        return true;
    }
}
```