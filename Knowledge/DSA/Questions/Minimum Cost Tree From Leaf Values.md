# Minimum Cost Tree From Leaf Values

```java
class Solution {
    public int mctFromLeafValues(int[] arr){
        Stack<Integer> st=new Stack<>();
        st.push(Integer.MAX_VALUE);

        int ans=0;

        for(int x:arr){
            while(st.peek()<=x){
                int mid=st.pop();
                ans+=mid*Math.min(st.peek(),x);
            }
            st.push(x);
        }

        while(st.size()>2)
            ans+=st.pop()*st.peek();

        return ans;
    }
}
```