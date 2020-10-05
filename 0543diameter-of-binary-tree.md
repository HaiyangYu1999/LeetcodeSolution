Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the **longest** path between any two nodes in a tree. This path may or may not pass through the root.

**Example:**
Given a binary tree

```
          1
         / \
        2   3
       / \     
      4   5    
```



Return **3**, which is the length of the path [4,2,1,3] or [5,2,1,3].

**Note:** The length of path between two nodes is represented by the number of edges between them.

## 1 递归

分为经过root和不经过root. 如果不经过root, 当前直径就等于左子树直径和右子树直径的最大者. 如果经过root, 直径就等于左子树高度+右子树高度. 三者取最大值即当前树的直径.

递归地这样写很简单, 但是太慢了. 每一次计算都要计算树的高度, 即都要遍历到树的叶子节点.

只超过了9%的时间.

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
    public int diameterOfBinaryTree(TreeNode root) {
        return (root == null) ? 0 : Math.max(treeDepth(root.left) + treeDepth(root.right), Math.max(diameterOfBinaryTree(root.left), diameterOfBinaryTree(root.right)));
    }
    private int treeDepth(TreeNode root){
        return (root == null) ? 0 : Math.max(treeDepth(root.left) + 1,treeDepth(root.right) + 1);
    }
}
```

## 2 原地修改树

首先写一个函数`int computeDepth(TreeNode root)`, 把树中的每一个节点的val值都换成以该节点作为根节点的子树的高度.

这样, 就方便多了.

对于每一个节点root, 

+ 如果root为null, 那么直接返回0就好了

+ 如果root的左侧或右侧为空(这里假设右侧为空), 根据经过根节点和不经过根节点的分类, 那么以root生成的子树的直径就为`Math.max(getDiameter(root.left), root.left.val)`. 这里的`root.left.val`为root的左子树的高度. `getDiameter(root.left)`为不经过根节点, `root.left.val`为经过根节点.
+ 如果root的两侧都非空, 还是按照经过根节点和不经过根节点来. `int diameterPassRoot = root.left.val + root.right.val;`和`int diameterNotPassRoot = Math.max(getDiameter(root.left), getDiameter(root.right));`,最后返回`Math.max(diameterPassRoot, diameterNotPassRoot);`

这个方法是提前整理出了任意一个节点的高度, 避免了方法1中的频繁计算树的高度的问题.

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
    public int diameterOfBinaryTree(TreeNode root) {
        computeDepth(root);
        return getDiameter(root);
    }
    private int getDiameter(TreeNode root)
    {
        if(root == null || root.left == null && root.right == null)
            return 0;
        if(root.left == null && root.right != null)
            return Math.max(getDiameter(root.right), root.right.val);
        if(root.left != null && root.right == null)
            return Math.max(getDiameter(root.left), root.left.val);
        int diameterPassRoot = root.left.val + root.right.val;
        int diameterNotPassRoot = Math.max(getDiameter(root.left), getDiameter(root.right));
        return Math.max(diameterPassRoot, diameterNotPassRoot);
    }
    private int computeDepth(TreeNode root)
    {
        if(root == null)
            return 0;
        root.val = Math.max(computeDepth(root.left), computeDepth(root.right)) + 1;
        return root.val;
    }
}
```

## 3 全局变量

贴一个来自leetcode官网上的方法. https://leetcode-cn.com/problems/diameter-of-binary-tree/solution/er-cha-shu-de-zhi-jing-by-leetcode-solution/

因为路径肯定是要经过某一个子树的根节点的. 所以找出所有的节点i, 计算每一个i的(左子树深度-1)和(右子树深度-1)的和, 取最大的那一个就可以了

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
    private int ans = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        getDepth(root);
        return ans;
    }
    private int getDepth(TreeNode root)
    {
        if(root == null)
            return 0;
        int L = getDepth(root.left);
        int R = getDepth(root.right);
        ans = Math.max(ans, L + R);
        return Math.max(L, R) + 1;
    }
    
}
```

