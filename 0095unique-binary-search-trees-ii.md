Given an integer `n`, generate all structurally unique **BST's** (binary search trees) that store values 1 ... *n*.

**Example:**

```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

 

**Constraints:**

- `0 <= n <= 8`

## 递归

首先, 和leetcode 96一样, 要先选取根节点. 根节点选定了, 所有的组合方式也就确定了.

> 举例 input = 6, 那么我们需要在(1,2,3,4,5,6)中选一个作为根节点. 不妨假设我们选择3作为根节点.
>
> 那么左子树就需要找到(1,2)对应的所有BST, 右子树就需要找到(4,5,6)对应的所有BST.
>
> 左子树好说, 就是`generateTrees(2)`返回的所有值. 
>
> **对于右子树, (4,5,6)构成的所有的BST形状上和(1,2,3)对应的所有BST一样, 只不过每个节点都加了3. **
>
> 也就是说, 找到了`generateTrees(3)`返回的所有子树的所有节点都加3, 就变成了所有右子树的BST. 
>
> 那么我们新构造一个函数`List<TreeNode> generateTrees(int n, int base)`, base是构造出从1到n的所有可能子树之后每个节点都加上base之后的所有子树. 
>
> 对于根节点3, 所有的可能方式就是`generateTrees(2, 0)`和`generateTrees(3, 3)`两者组合一下的结果.
>
> 然后遍历根节点, 从1到6, 就找出所有可能方式了.

计算的过程中发现了很多重复计算, 比如计算`generateTrees(5)`, 当选取3为根节点时, 要计算`generateTrees(2, 0)`和`generateTrees(2, 2)`. 这两个返回的结果**形状上**完全一样, 只不过后一个每一个节点都比前一个对应的节点多了2.

所以可以不用递归, 打表储存从1到i的所有BST的形状即可, `dp[i] = generateTrees(i)` 用到的时候拿出来再在每个节点上加上base即可.

但是他给的这个TreeNode类没有实现clone()方法, 所以就没有试.

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
    public List<TreeNode> generateTrees(int n) {
        if(n == 0)
            return new ArrayList<TreeNode>();
        return generateTrees(n, 0);
    }
    private List<TreeNode> generateTrees(int n, int base)
    {
        List<TreeNode> trees = new ArrayList<>();
        if(n == 0)
        {
            trees.add(null);
            return trees;
        }
            
        if(n == 1)
        {
            trees.add(new TreeNode(1 + base));
            return trees;
        }
        for(int i = 1; i <= n; ++i)
        {
            List<TreeNode> leftTrees = generateTrees(i - 1, 0 + base);
            List<TreeNode> rightTrees = generateTrees(n - i, i + base);
            for(TreeNode leftTree : leftTrees)
            {
                for(TreeNode rightTree : rightTrees)
                {
                    TreeNode rootWithi = new TreeNode(i + base);
                    rootWithi.left = leftTree;
                    rootWithi.right = rightTree;
                    trees.add(rootWithi);
                }
            }
        }
        return trees;
    }
}
```

