# Check if All Characters Have Equal Number of Occurrences

```java
class Solution {
    public boolean areOccurrencesEqual(String s){
        int[] freq=new int[26];

        for(char ch:s.toCharArray())
            freq[ch-'a']++;

        int count=0;

        for(int x:freq){
            if(x==0) continue;
            if(count==0) count=x;
            else if(count!=x) return false;
        }

        return true;
    }
}
```