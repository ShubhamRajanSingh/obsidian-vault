# Determine if String Halves Are Alike

```java
class Solution {
    public boolean halvesAreAlike(String s){
        int count=0;
        int n=s.length();

        for(int i=0;i<n/2;i++)
            if(vowel(s.charAt(i))) count++;

        for(int i=n/2;i<n;i++)
            if(vowel(s.charAt(i))) count--;

        return count==0;
    }

    boolean vowel(char ch){
        return "aeiouAEIOU".indexOf(ch)>=0;
    }
}
```