Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

 

**Example 1:**

```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

**Example 2:**

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

**Follow up:**
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

 

**Constraints:**

- The number of elements of the BST is between `1` to `10^4`.
- You may assume `k` is always valid, `1 ≤ k ≤ BST's total elements`.

## 非递归中序遍历

简单来说, 这题就是找到BST中序遍历的第k个值.

由于递归方法不太方便传入一个全局计数器, 所以采用非递归中序遍历.

维持一个变量currRank, 每遍历一个元素就++currRank.

当currRank = k的时候就返回

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
    public int kthSmallest(TreeNode root, int k) {
        int currRank = 0;
        Deque<TreeNode> stk = new ArrayDeque<>();
        TreeNode tmp = root;
        while(tmp != null)
        {
            stk.offerFirst(tmp);
            tmp = tmp.left;
        }
        while(!stk.isEmpty())
        {
            TreeNode curr = stk.pollFirst();
            ++currRank;
            if(currRank == k)
                return curr.val;
            TreeNode tmp1 = curr.right;
            while(tmp1 != null)
            {
                stk.offerFirst(tmp1);
                tmp1 = tmp1.left;
            }
        }
        return -1; // if k > tree.size, return -1
    }
}
```

