Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

**Note:** A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,

```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
```

Return:

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

## DFS

直接深度优先遍历就可以了. 

注意递归边界是叶子节点. 当叶子节点的值与sum相等时, 将递归路径加入结果中. 注意要加入递归路径的拷贝. 因为递归路径随时在变化.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        if(root == null)
            return new ArrayList();
        List<List<Integer>> lists = new ArrayList<>();
        List<Integer> curr = new ArrayList<>();
        pathSum(lists, curr, root, sum);
        return lists;
        
    }
    private void pathSum(List<List<Integer>> ans, List<Integer> curr, TreeNode root, int sum)
    {
        if(root == null)
            throw new RuntimeException("null pointer");
        if(root.left == null && root.right == null)
        {
            if(root.val == sum)
            {
                curr.add(root.val);
                ans.add(new ArrayList<Integer>(curr));
                curr.remove(curr.size() - 1);
            }
        }
        else if(root.left == null && root.right != null)
        {
            curr.add(root.val);
            pathSum(ans, curr, root.right, sum - root.val);
            curr.remove(curr.size() - 1);
        }
        else if(root.left != null && root.right == null)
        {
            curr.add(root.val);
            pathSum(ans, curr, root.left, sum - root.val);
            curr.remove(curr.size() - 1);
        }
        else
        {
            curr.add(root.val);
            pathSum(ans, curr, root.right, sum - root.val);
            pathSum(ans, curr, root.left, sum - root.val);
            curr.remove(curr.size() - 1);
        }
    }
}
```

