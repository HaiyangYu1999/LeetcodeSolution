Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

 



**Example:**

**![img](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)**

```
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
```

 

**Note:**

- `next()` and `hasNext()` should run in average O(1) time and uses O(*h*) memory, where *h* is the height of the tree.
- You may assume that `next()` call will always be valid, that is, there will be at least a next smallest number in the BST when `next()` is called.

## 非递归形式的中序遍历

其实就是把中序遍历非递归形式拆解下来. 

首先令tmp = root, 然后循环的进行push(tmp), tmp = tmp->left. 直到tmp为null. 初始工作结束.

每从栈中取出一个值top, 返回top->val, 并且令tmp = top->right. 然后循环的进行push(tmp), tmp = tmp->left.

当栈为空时即可完成中序遍历的过程. 由于只push了n个节点, 所以平均时间复杂度是O(1). 栈中保存的最大也只是二叉树的高度. 满足空间复杂度O(h).

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
class BSTIterator {
    Deque<TreeNode> stk;
    public BSTIterator(TreeNode root) {
        stk = new ArrayDeque<TreeNode>();
        TreeNode tmp = root;
        while(tmp != null)
        {
            stk.offerFirst(tmp);
            tmp = tmp.left;
        }
    }
    
    /** @return the next smallest number */
    public int next() {
        TreeNode top = stk.pollFirst();
        int ans = top.val;
        TreeNode tmp = top.right;
        while(tmp != null)
        {
            stk.offerFirst(tmp);
            tmp = tmp.left;
        }
        return ans;
    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stk.isEmpty();
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

