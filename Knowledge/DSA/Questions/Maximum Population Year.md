# Maximum Population Year

```java
class Solution {
    public int maximumPopulation(int[][] logs){
        int[] years=new int[101];

        for(int[] log:logs){
            years[log[0]-1950]++;
            years[log[1]-1950]--;
        }

        int max=0,year=1950,curr=0;

        for(int i=0;i<101;i++){
            curr+=years[i];

            if(curr>max){
                max=curr;
                year=1950+i;
            }
        }

        return year;
    }
}
```