# Minimum Add to Make Parentheses Valid

## Tags
- dsa
- stack
- greedy
- strings
- leetcode

```java
class Solution {
    public int minAddToMakeValid(String s) {
        int balance = 0;
        int additions = 0;

        for(char ch : s.toCharArray()) {
            if(ch == '(') {
                balance++;
            } else {
                if(balance == 0) {
                    additions++;
                } else {
                    balance--;
                }
            }
        }

        return additions + balance;
    }
}
```
