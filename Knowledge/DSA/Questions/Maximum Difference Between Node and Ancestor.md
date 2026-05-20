# Maximum Difference Between Node and Ancestor

## Tags
- dsa
- trees
- dfs
- leetcode

```java
class Solution {
    public int maxAncestorDiff(TreeNode root) {
        return dfs(root, root.val, root.val);
    }

    private int dfs(TreeNode node,int min,int max){
        if(node == null) return max - min;

        min = Math.min(min,node.val);
        max = Math.max(max,node.val);

        return Math.max(
            dfs(node.left,min,max),
            dfs(node.right,min,max)
        );
    }
}
```
