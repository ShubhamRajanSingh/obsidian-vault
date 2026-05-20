# Maximum Repeating Substring

```java
class Solution {
    public int maxRepeating(String sequence,String word){
        int ans=0;
        String cur=word;

        while(sequence.contains(cur)){
            ans++;
            cur+=word;
        }

        return ans;
    }
}
```