# Decode String

## Tags
- dsa
- stack
- strings
- recursion
- leetcode

```java
class Solution {
    public String decodeString(String s) {
        Stack<Integer> counts = new Stack<>();
        Stack<StringBuilder> result = new Stack<>();

        StringBuilder current = new StringBuilder();
        int k = 0;

        for(char ch : s.toCharArray()) {
            if(Character.isDigit(ch)) {
                k = k * 10 + (ch - '0');
            } else if(ch == '[') {
                counts.push(k);
                result.push(current);
                current = new StringBuilder();
                k = 0;
            } else if(ch == ']') {
                StringBuilder temp = current;
                current = result.pop();

                int repeat = counts.pop();

                while(repeat-- > 0) {
                    current.append(temp);
                }
            } else {
                current.append(ch);
            }
        }

        return current.toString();
    }
}
```
