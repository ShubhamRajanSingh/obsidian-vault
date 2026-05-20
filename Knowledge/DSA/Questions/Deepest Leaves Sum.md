# Deepest Leaves Sum

```java
class Solution {
    public int deepestLeavesSum(TreeNode root){
        Queue<TreeNode> q=new LinkedList<>();
        q.offer(root);

        int sum=0;

        while(!q.isEmpty()){
            int size=q.size();
            sum=0;

            while(size-- > 0){
                TreeNode node=q.poll();
                sum+=node.val;

                if(node.left!=null) q.offer(node.left);
                if(node.right!=null) q.offer(node.right);
            }
        }

        return sum;
    }
}
```