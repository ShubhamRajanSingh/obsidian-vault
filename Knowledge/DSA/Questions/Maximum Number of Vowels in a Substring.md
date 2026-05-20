# Maximum Number of Vowels in a Substring

```java
class Solution {
    public int maxVowels(String s,int k){
        int count=0,ans=0;

        for(int i=0;i<s.length();i++){
            if(vowel(s.charAt(i))) count++;

            if(i>=k&&vowel(s.charAt(i-k))) count--;

            ans=Math.max(ans,count);
        }

        return ans;
    }

    boolean vowel(char ch){
        return "aeiou".indexOf(ch)>=0;
    }
}
```