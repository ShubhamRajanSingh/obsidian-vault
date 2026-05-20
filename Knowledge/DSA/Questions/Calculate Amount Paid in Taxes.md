# Calculate Amount Paid in Taxes

```java
class Solution {
    public double calculateTax(int[][] brackets,int income){
        double ans=0;
        int prev=0;

        for(int[] b:brackets){
            int upper=b[0],percent=b[1];
            int taxable=Math.min(income,upper)-prev;

            if(taxable>0)
                ans+=taxable*percent/100.0;

            prev=upper;

            if(income<=upper) break;
        }

        return ans;
    }
}
```