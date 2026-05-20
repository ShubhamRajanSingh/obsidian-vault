# Smallest String Starting From Leaf

## Tags
- dsa
- trees
- dfs
- strings
- leetcode

```java
class Solution {
    String ans = "~";

    public String smallestFromLeaf(TreeNode root) {
        dfs(root, new StringBuilder());
        return ans;
    }

    private void dfs(TreeNode node, StringBuilder sb){
        if(node == null) return;

        sb.append((char)(node.val + 'a'));

        if(node.left == null && node.right == null){
            String cur = sb.reverse().toString();
            sb.reverse();
            if(cur.compareTo(ans) < 0) ans = cur;
        }

        dfs(node.left,sb);
        dfs(node.right,sb);

        sb.deleteCharAt(sb.length()-1);
    }
}
```
