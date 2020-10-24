Given a binary tree, return all root-to-leaf paths.

**Note:** A leaf is a node with no children.

**Example:**

```
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3
```

## DFS

直接DFS到每一个叶子节点即可

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
    public List<String> binaryTreePaths(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        List<String> ans1 = new ArrayList<>();
        if(root == null)
            return ans1;
        List<Integer> curr = new ArrayList<>();
        binaryTreePaths(ans, curr, root);
        
        for(List<Integer> integers : ans)
        {
            ans1.add(integersToString(integers));
        }
        return ans1;
    }
    private void binaryTreePaths(List<List<Integer>> ans, List<Integer> curr, TreeNode root)
    {
        if(root.left == null && root.right == null)
        {
            curr.add(root.val);
            ans.add(new ArrayList<>(curr));
            curr.remove(curr.size() - 1);
            return;
        }
        if(root.left != null)
        {
            curr.add(root.val);
            binaryTreePaths(ans, curr, root.left);
            curr.remove(curr.size() - 1);
        }
        if(root.right != null)
        {
            curr.add(root.val);
            binaryTreePaths(ans, curr, root.right);
            curr.remove(curr.size() - 1);
        }
    }
    
    private String integersToString(List<Integer> integers)
    {
        StringBuilder s = new StringBuilder();
        for(int i = 0; i < integers.size(); ++i)
        {
            s.append(integers.get(i));
            if(i != integers.size() - 1)
                s.append("->");
        }
        return s.toString();
    }
}
```

