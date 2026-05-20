# Pancake Sorting

## Tags
- dsa
- sorting
- greedy
- leetcode

```java
class Solution {
    public List<Integer> pancakeSort(int[] arr) {
        List<Integer> ans = new ArrayList<>();

        for(int size = arr.length; size > 1; size--) {
            int idx = 0;

            for(int i = 0; i < size; i++) {
                if(arr[i] > arr[idx]) idx = i;
            }

            reverse(arr, idx);
            ans.add(idx + 1);

            reverse(arr, size - 1);
            ans.add(size);
        }

        return ans;
    }

    private void reverse(int[] arr, int end){
        int start = 0;

        while(start < end){
            int temp = arr[start];
            arr[start++] = arr[end];
            arr[end--] = temp;
        }
    }
}
```
