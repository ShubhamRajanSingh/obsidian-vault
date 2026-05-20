# Letter Case Permutation

## Tags
- dsa
- backtracking
- strings
- recursion
- leetcode

```java
class Solution {
    public List<String> letterCasePermutation(String s) {
        List<String> ans = new ArrayList<>();

        dfs(s.toCharArray(), 0, ans);

        return ans;
    }

    private void dfs(char[] arr,
                     int idx,
                     List<String> ans) {

        if(idx == arr.length) {
            ans.add(new String(arr));
            return;
        }

        if(Character.isLetter(arr[idx])) {
            arr[idx] = Character.toLowerCase(arr[idx]);
            dfs(arr, idx + 1, ans);

            arr[idx] = Character.toUpperCase(arr[idx]);
            dfs(arr, idx + 1, ans);
        } else {
            dfs(arr, idx + 1, ans);
        }
    }
}
```
