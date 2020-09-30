Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

 

But the following `[1,2,2,null,3,null,3]` is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

 

**Follow up:** Solve it both recursively and iteratively.

## 1 递归

两个树tree1, tree2对称 => 两个树的根节点相同, 并且tree1的左子树与tree2的右子树对称, 并且tree1的右子树和tree2的左子树对称.

当一个树的左右子树对称时, 这个树是对称的

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
    public boolean isSymmetric(TreeNode root) {
        if(root == null)
            return true;
        return isTwoTreeSymmetric(root.left, root.right);
    }
    private boolean isTwoTreeSymmetric(TreeNode tree1, TreeNode tree2)
    {
        if(tree1 == null && tree2 == null)
            return true;
        else if(tree1 != null && tree2 == null || tree1 == null && tree2 != null)
            return false;
        else
        {
            return tree1.val == tree2.val && isTwoTreeSymmetric(tree1.left, tree2.right) && isTwoTreeSymmetric(tree1.right, tree2.left);
        }
    }
}
```

## 2 迭代

这个题要利用队列.

首先将root.left和root.right加入到队列中,

然后当队列非空时, 每次从队列中取出2个指针tree1, tree2. 如果tree1.val=tree2.val, 那么就可以将tree1.left和tree2.right**连续的**加入队列, 在将来某个时刻这两个值也会被连续的读出, 判断tree1.left和tree2.right是不是对称的. 同理, 也需要将tree1.right和tree2.left**连续的**加入队列. 

如果tree1和tree2都是null, 那可以跳过他们, 再读下一对数据.

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
    public boolean isSymmetric(TreeNode root) {
        if(root == null)
            return true;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root.left);
        q.offer(root.right);
        while(!q.isEmpty())
        {
            TreeNode tree1 = q.poll();
            TreeNode tree2 = q.poll();
            if(tree1 == null && tree2 == null)
                continue;
            else if(tree1 != null && tree2 == null || tree1 == null && tree2 != null)
                return false;
            else
            {
                if(tree1.val != tree2.val)
                    return false;
                else
                {
                    q.offer(tree1.left);
                    q.offer(tree2.right);
                    q.offer(tree1.right);
                    q.offer(tree2.left);
                }
            }
        }
        return true;
    }
    
}
```

