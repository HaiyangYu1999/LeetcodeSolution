Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

**Example 2:**

```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-1000 <= Node.val <= 1000`

## 递归

一开始刚看这个题目, 感觉很简单, `return root == null ? 0 : Math.min(minDepth(root.left), minDepth(root.right)) + 1;` 一行就能搞定. submit后才发现根本不是这样. 这题和求maxDepth的思路不一样!!!

考虑下面的树， 上面的“一行算法”就失效了

```
1
 \
  2
   \
    3
```

正确的解法应该是这样的

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
    public int minDepth(TreeNode root) {
        if(root == null)
            return 0;
        else if(root.left == null && root.right != null)
            return minDepth(root.right) + 1;
        else if(root.left != null && root.right == null)
            return minDepth(root.left) + 1;
        else 
            return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
}
```

