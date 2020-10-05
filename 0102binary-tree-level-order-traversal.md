Given a binary tree, return the *level order* traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```



return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```

## 队列 + BFS

利用队列.

对于每一层的节点, 先将这一层的所有节点入队列, 并且求出这一层所有节点的个数thisLevelSize.

对于队列中每一个节点, 出队列这个节点, 并将非空的左右子节点入队列.

出队列thisLevelSize次, 这一层就遍历完了. 放入到一个list即可.

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
    public List<List<Integer>> levelOrder(TreeNode root) {
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
        return llist;
    }
}
```

