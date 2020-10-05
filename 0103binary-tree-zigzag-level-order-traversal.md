Given a binary tree, return the *zigzag level order* traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```



return its zigzag level order traversal as:

```
[
  [3],
  [20,9],
  [15,7]
]
```

## 队列 + BFS

和leetcode 102 一样, 只是增加一个变量currentDepth来判断是从左到右还是从右到左.

这时, 为了实现从右到左的效果, 我们可以用LinkedList的push和add方法. 一个头插一个尾插. (注意只有LinkedList有这个操作, List接口是没有的)

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> llist = new ArrayList<>();
        if(root == null)
            return llist;
        int currentDepth = 1;
        Queue<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        while(!q.isEmpty())
        {
            int thisLevelSize = q.size();
            LinkedList<Integer> thisLevelList = new LinkedList<>();
            while(thisLevelSize != 0)
            {
                TreeNode tmp = q.poll();
                if((currentDepth & 1) == 0)
                    thisLevelList.push(tmp.val);
                else
                    thisLevelList.add(tmp.val);
                if(tmp.left != null)
                    q.offer(tmp.left);
                if(tmp.right != null)
                    q.offer(tmp.right);
                --thisLevelSize;
            }
            ++currentDepth;
            llist.add(thisLevelList);
        }
        return llist;
    }
}
```

