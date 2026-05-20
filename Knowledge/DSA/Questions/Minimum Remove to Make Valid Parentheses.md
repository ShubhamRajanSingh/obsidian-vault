# Minimum Remove to Make Valid Parentheses

```java
class Solution {
    public String minRemoveToMakeValid(String s){
        Stack<Integer> st=new Stack<>();
        Set<Integer> remove=new HashSet<>();

        for(int i=0;i<s.length();i++){
            char ch=s.charAt(i);

            if(ch=='(') st.push(i);
            else if(ch==')'){
                if(st.isEmpty()) remove.add(i);
                else st.pop();
            }
        }

        while(!st.isEmpty()) remove.add(st.pop());

        StringBuilder sb=new StringBuilder();

        for(int i=0;i<s.length();i++)
            if(!remove.contains(i)) sb.append(s.charAt(i));

        return sb.toString();
    }
}
```