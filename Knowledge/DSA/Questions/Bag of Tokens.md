# Bag of Tokens

## Tags
- dsa
- greedy
- two-pointers
- arrays
- leetcode

```java
class Solution {
    public int bagOfTokensScore(int[] tokens, int power) {
        Arrays.sort(tokens);

        int left = 0;
        int right = tokens.length - 1;

        int score = 0;
        int ans = 0;

        while(left <= right) {
            if(power >= tokens[left]) {
                power -= tokens[left++];
                score++;
                ans = Math.max(ans, score);
            } else if(score > 0) {
                power += tokens[right--];
                score--;
            } else {
                break;
            }
        }

        return ans;
    }
}
```
