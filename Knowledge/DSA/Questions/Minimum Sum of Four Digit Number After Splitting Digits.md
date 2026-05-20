# Minimum Sum of Four Digit Number After Splitting Digits

```java
class Solution {
    public int minimumSum(int num){
        char[] arr=String.valueOf(num).toCharArray();
        Arrays.sort(arr);

        int a=(arr[0]-'0')*10+(arr[2]-'0');
        int b=(arr[1]-'0')*10+(arr[3]-'0');

        return a+b;
    }
}
```