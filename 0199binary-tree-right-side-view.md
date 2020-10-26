Given a binary tree, imagine yourself standing on the *right* side of it, return the values of the nodes you can see ordered from top to bottom.

**Example:**

```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

## 1 层序遍历

没什么好解释的, 就直接把每层的最后一个add进结果中就ok了

但是空间复杂度高, 为O(n), n是二叉树中最多的一层的节点个数

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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        Queue<TreeNode> q = new ArrayDeque<>();
        if(root == null)
            return ans;
        q.offer(root);
        while(!q.isEmpty())
        {
            int size = q.size();
            while(size != 0)
            {
                TreeNode tmp = q.poll();
                if(size == 1)
                    ans.add(tmp.val);
                --size;
                if(tmp.left != null)
                    q.offer(tmp.left);
                if(tmp.right != null)
                    q.offer(tmp.right);
            }
        }
        return ans;
    }
}
```

## 2 改进的中序遍历

对于下面的二叉树

```
      1
     / \
    2   3
   /\    \
  4  5    6
 /
7
```

如果按照**每层最右边的节点先于左侧的节点得到遍历**, 那么问题就解决了.

因为可以设置一个计数currDepth, 遍历的时候遍历该点的值和计数.

**如果遍历的计数大于list的长度, 就说明这一层还没有任何一个节点加到list中.**

再根据遍历的规则, 每层最右边的节点要先于左边的节点, 所以最右边的节点会被add进list中, 此时list长度增加1.

然后这一层左边的那些节点再次判断的时候, 就不满足currDepth大于list的长度了, 不会被计算进去.

这样的遍历怎么实现呢, 根节点->右子树->左子树这样的方式就可以实现了.

空间复杂度O(m), m 是树的深度. **通常情况下**比上面的BFS复杂度要低. 极端情况下不一定.

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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if(root == null)
            return ans;
        dfs(ans, root, 1);
        return ans;
    }
    private void dfs(List<Integer> ans, TreeNode root, int currDepth)
    {
        if(root == null)
            return;
        if(currDepth > ans.size())
        {
            ans.add(root.val);
        }
        dfs(ans, root.right, currDepth + 1);
        dfs(ans, root.left, currDepth + 1);
    }
}
```

