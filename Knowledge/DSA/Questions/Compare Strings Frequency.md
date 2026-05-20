# Compare Strings Frequency

```java
class Solution {
    public int[] numSmallerByFrequency(String[] q,String[] w){
        int[] arr=new int[w.length];

        for(int i=0;i<w.length;i++)
            arr[i]=freq(w[i]);

        Arrays.sort(arr);

        int[] ans=new int[q.length];

        for(int i=0;i<q.length;i++){
            int f=freq(q[i]);
            int idx=upper(arr,f);
            ans[i]=arr.length-idx;
        }

        return ans;
    }

    int freq(String s){
        char c='z';
        int count=0;

        for(char ch:s.toCharArray()){
            if(ch<c){c=ch;count=1;}
            else if(ch==c) count++;
        }

        return count;
    }

    int upper(int[] arr,int t){
        int l=0,r=arr.length;

        while(l<r){
            int m=(l+r)/2;
            if(arr[m]<=t) l=m+1;
            else r=m;
        }

        return l;
    }
}
```