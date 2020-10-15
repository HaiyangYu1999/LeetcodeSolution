Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

**Example:**

```
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

## 递归

这题很简单, 先找到数组中间的元素mid作为根节点, 然后问题就变成了0到mid-1的子数组构造BST和mid+1到length-1的子数组构造BST了. 递归地进行即可.

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
    public TreeNode sortedArrayToBST(int[] nums) {
        return sortedArrayToBST(nums, 0, nums.length - 1);
    }
    private TreeNode sortedArrayToBST(int[] nums, int begin, int end)
    {
        if(begin > end)
            return null;
        else if(begin == end)
            return new TreeNode(nums[begin]);
        else
        {
            int mid = begin + (end - begin) / 2;
            return new TreeNode(nums[mid], sortedArrayToBST(nums, begin, mid - 1), sortedArrayToBST(nums, mid + 1, end));
        }
    }
}
```

