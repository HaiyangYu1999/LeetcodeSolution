Given a binary tree, return the *bottom-up level order* traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```



return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]
```

## BST

和leetcode 102 完全一样, 只不过最后要把list翻转一下.

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> llist = new ArrayList<>();
        if(root == null)
            return llist;
        Queue<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        while(!q.isEmpty())
        {
            int thisLevelSize = q.size();
            List<Integer> thisLevelList = new ArrayList<>();
            while(thisLevelSize != 0)
            {
                TreeNode tmp = q.poll();
                thisLevelList.add(tmp.val);
                if(tmp.left != null)
                    q.offer(tmp.left);
                if(tmp.right != null)
                    q.offer(tmp.right);
                --thisLevelSize;
            }
            llist.add(thisLevelList);
        }
        Collections.reverse(llist);
        return llist;
    }
}
```

