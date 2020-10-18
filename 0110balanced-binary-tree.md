Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of *every* node differ in height by no more than 1.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```

**Example 3:**

```
Input: root = []
Output: true
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-104 <= Node.val <= 104`

## 自底向上的递归

碰到不平衡的节点就返回高度为-1.

当两个子树都平衡的时候才计算高度

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
    private final static int NotBalanced = -1;
    public boolean isBalanced(TreeNode root) {
        return getDepth(root)[1] != NotBalanced;
    }
    
    private int[] getDepth(TreeNode root)
    {
        if(root == null)
            return new int[]{0,0};
        if(root.left == null && root.right == null)
            return new int[]{1,0};
        int[] leftTree = getDepth(root.left);
        if(leftTree[1] == NotBalanced)
            return new int[]{0, NotBalanced};
        int[] rightTree = getDepth(root.right);
        if(rightTree[1] == NotBalanced)
            return new int[]{0, NotBalanced};
        if(Math.abs(leftTree[0] - rightTree[0]) > 1)
            return new int[]{0, NotBalanced};
        return new int[]{Math.max(leftTree[0], rightTree[0]) + 1, 0};
    }
}
```

