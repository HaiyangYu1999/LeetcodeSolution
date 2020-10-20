Given a binary tree containing digits from `0-9` only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path `1->2->3` which represents the number `123`.

Find the total sum of all root-to-leaf numbers.

**Note:** A leaf is a node with no children.

**Example:**

```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```

**Example 2:**

```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

## DFS

直接深度优先搜索到所有的叶子节点, 然后把这些叶子节点上的值都加起来即可.

可惜java没法传引用, 只能定制一个包装类IntegerWrapper. 用这种方式实现int参数的修改

如果是c++的话会方便得多. 直接`private void sumNumbers(int& sum, int& curr, TreeNode* root)`就完事了

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
    public int sumNumbers(TreeNode root) {
        IntegerWrapper sum = new IntegerWrapper();
        IntegerWrapper curr = new IntegerWrapper();
        sumNumbers(sum, curr, root);
        return sum.get();
    }
    
    private void sumNumbers(IntegerWrapper sum, IntegerWrapper curr, TreeNode root)
    {
        if(root == null)
            return;
        else if(root.left == null && root.right == null)
        {
            curr.set(curr.get() * 10);
            curr.set(curr.get() + root.val);
            sum.set(sum.get() + curr.get());
            curr.set(curr.get() / 10);
        }
        else{
            curr.set(curr.get() * 10);
            curr.set(curr.get() + root.val);
            if(root.left != null)
                sumNumbers(sum, curr, root.left);
            if(root.right != null)
                sumNumbers(sum, curr, root.right);
            curr.set(curr.get() / 10);
        }
    }
    
    private static class IntegerWrapper
    {
        private Integer i = 0;
        public void set(int i)
        {
            this.i = Integer.valueOf(i);
        }
        public int get()
        {
            return i;
        }
    }
}
```

