The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

**Example 1:**

```
Input: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

Output: 7 
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

**Example 2:**

```
Input: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```

## 1 动态规划

首先这题递归肯定是能做的. 动态规划的思想也是从递归来的. 

> 递归: 记f`(root)`为以root作为根节点的子树全部抢劫的最大金额. 那么对于root来说, 可以抢root, 那么就不能抢`root->left`和`root->right`了, 只能从`root->left->left(right)`和`root->right->left(right)`来抢.
>
> 即 `robRootAndGrandchildren = root.val + f(root->left->left) + f(root->left->right) + f(root->right->left) + f(root->right->right)`
>
> 也可以选择不抢root, 那么root->left和root->right对应的子树就随便抢了
>
> `robChildren = f(root->left) + f(root->right)`
>
> 最后取两者最大值 `f(root) = max(robChildren, robRootAndGrandChildren)`

但是如果这样直接递归的话会有大量的重复计算. 例如, 计算`robRootAndGrandchildren`的时候会计算`f(root->left->left)`, 计算`robChildren`的时候由于要计算`f(root->left)`所以也会间接的计算`f(root->left->left)`.

需要用动态规划的思路, **从底向上的计算**, 并且把计算结果保存起来. 这时, 可以选择hashmap来做一个TreeNode到Integer的映射.

也可以在树上本身存储. 方法是, 树的某个节点Node的val域存储以Node作为根节点的子树所能抢劫的最大值.

这样, 再计算`f(root->right->right)`的时候就只需要获取`root->right->right->val`即可

我们用后序遍历, 因为这样可以保证孩子节点的抢劫最大值先于父节点计算. (计算父节点的抢劫最大值需要孩子节点和孙子节点的抢劫最大值)

用后序遍历一遍之后直接返回根节点的val即可

```java
//Runtime: 0 ms, faster than 100.00% of Java online submissions for House Robber III.
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
    public int rob(TreeNode root) {
        if(root == null)
            return 0;
        postTraverse(root);
        return root.val;
    }
    private void postTraverse(TreeNode root)
    {
        if(root == null)
            return;
        postTraverse(root.left);
        postTraverse(root.right);
        getMax(root);
    }
    private void getMax(TreeNode root)
    {
        if(root == null)
            return;
        else if(root.left == null && root.right == null)
        {
            root.val = root.val;
            return;
        }
        // sum of root's children
        int childSum = ((root.left == null) ? 0 : root.left.val) + ((root.right == null) ? 0 : root.right.val);
        // sum of root's grandchildren
        int grandchildSum = 0;
        if(root.left != null)
        {
            grandchildSum += (root.left.left == null) ? 0 : root.left.left.val;
            grandchildSum += (root.left.right == null) ? 0 : root.left.right.val;
        }
        if(root.right != null)
        {
            grandchildSum += (root.right.left == null) ? 0 : root.right.left.val;
            grandchildSum += (root.right.right == null) ? 0 : root.right.right.val;
        }
        root.val = Math.max(childSum, root.val + grandchildSum);
    }
}
```

## 2 树形dp

思路参考于https://leetcode-cn.com/problems/house-robber-iii/solution/san-chong-fang-fa-jie-jue-shu-xing-dong-tai-gui-hu/

现在再来研究一下递归为什么慢, 是因为在计算root的时候既用到了root的孩子, 又用到了root的孙子. 而计算root的孩子的时候还要计算root的孙子. 所以造成了大量的重复计算. 如果我们把root孙子的信息存储到root孩子上面, 那么计算root只需要root孩子, 就避免了重复计算.

举个例子, `f(n) = f(n-1) + f(n-2)`进行递归的时候, f(n-2)计算了两次. 一次是计算f(n)的时候计算的, 一次是计算f(n-1)的时候计算的. 复杂度自然为O(2^n)

如果我们把`f(n-1)`上添加`f(n-2)`的信息, 使得计算`f(n)`的时候不需要再计算一遍`f(n-2)`, 而是直接从`f(n-1)`中提取`f(n-2)`. 那么复杂度自然就变为O(n)了

> `g(1) = [f(0), f(1)]`
>
> ...
>
> `g(n-1) = [f(n-2), f(n-1)]`
>
> `g(n) = [g(n-1)[1], g(n-1)[0] + g(n-1)[1]]`
>
> 这样求`g(n)`只需要知道`g(n-1)`

弄懂上面例子的原理后, 我们就可以开始了.

也是创建一个二维数组. 对于节点node作为根节点对应的子树来说,

root[0]代表从两个儿子开始抢的最大值

root[1]表示从四个孙子开始抢的最大值, 加上抢root本身的钱

root作为根节点生成的子树最大的抢劫和即为`max(root[0], root[1])`

最后返回这个二维数组`[root[0], root[1]]`. 而不是返回`max(root[0], root[1])`. **否则计算root的父节点rootFather时, 如果抢劫策略是抢劫根节点rootFather并且从孙子开始抢, 还是需要计算他的孙子(即root的儿子)的抢劫和, 即root[0]. 又产生了重复计算**

计算root[0]和root[1]的时候, root0显然是等于左子树能抢的最大值 加上 右子树能抢的最大值. 

`ans[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);`

对于root1, 即抢根节点加上四个孙子节点. 对于root的四个孙子节点的和, 就是root.left的两个儿子节点的和和root.right的两个儿子节点的和.

`ans[1] = root.val + left[0] + right[0]`

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
    public int rob(TreeNode root) {
        int[] ans = rob0(root);
        return Math.max(ans[0], ans[1]);
    }
    private int[] rob0(TreeNode root)
    {
        if(root == null)
            return new int[2];
        int[] left = rob0(root.left);
        int leftmax = Math.max(left[0], left[1]);
        int[] right = rob0(root.right);
        int rightmax = Math.max(right[0], right[1]);
        int[] ans = new int[2];
        ans[0] = leftmax + rightmax;
        ans[1] = root.val + left[0] + right[0];
        return ans;
    }
}
```

