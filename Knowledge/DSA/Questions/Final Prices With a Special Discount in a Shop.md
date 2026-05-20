# Final Prices With a Special Discount in a Shop

```java
class Solution {
    public int[] finalPrices(int[] prices){
        Stack<Integer> st=new Stack<>();

        for(int i=0;i<prices.length;i++){
            while(!st.isEmpty()&&prices[st.peek()]>=prices[i]){
                prices[st.pop()]-=prices[i];
            }

            st.push(i);
        }

        return prices;
    }
}
```