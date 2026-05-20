# Check Whether Two Strings are Almost Equivalent

```java
class Solution {
    public boolean checkAlmostEquivalent(String w1,String w2){
        int[] freq=new int[26];

        for(char ch:w1.toCharArray()) freq[ch-'a']++;
        for(char ch:w2.toCharArray()) freq[ch-'a']--;

        for(int x:freq)
            if(Math.abs(x)>3) return false;

        return true;
    }
}
```