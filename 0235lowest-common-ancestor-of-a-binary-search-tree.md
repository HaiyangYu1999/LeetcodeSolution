Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [2,1], p = 2, q = 1
Output: 2
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[2, 105]`.
- `-109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the BST.

## 1 hashmap

其实第一眼我看成了求一颗普通的二叉树的两个节点最近公共祖先节点了. **可能是受到剑指offer里面最后一题的影响吧, 在那里面的题就是普通树而不是BST. 再次提醒我们做题前一定要看清题意, 不要想当然**

储存每个孩子节点的父节点. 这样问题就变成了两条相交链表求第一个交点了. 但是又慢空间占用也大

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    private Map<TreeNode, TreeNode> map = new HashMap<>();
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        initMap(root);
        map.put(root, null);
        TreeNode a = p;
        TreeNode b = q;
        while(a != b)
        {
            a = (map.get(a) == null) ? q : map.get(a);
            b = (map.get(b) == null) ? p : map.get(b);
        }
        return a;
        
    }
    private void initMap(TreeNode root)
    {
        if(root == null || root.left == null && root.right == null)
            return;
        if(root.left != null)
        {
            map.put(root.left, root);
            initMap(root.left);
        }
        if(root.right != null)
        {
            map.put(root.right, root);
            initMap(root.right);
        }
    }
    private TreeNode getAncestor(TreeNode child)
    {
        return map.get(child);
    }
}
```

## 2. 递归

其实利用BST的条件之后就很简单了.

分类讨论如下:

+ 如果p, q有一个节点等于当前的节点root, 那么就返回root即可.

+ 如果p, q的值在root两侧, 那么公共节点也肯定是root. 因为从root开始这两个节点一左一右的就分道扬镳了

+ 如果p, q的值都小于root, 那么他们的最近公共祖先肯定至少是root.left, 所以从root.left开始找. 如果都大于root, 同理, 从root.right开始找

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr)
            return root;
        int rootVal = root->val;
        if(p->val == rootVal || q->val == rootVal)
        {
            return root;
        }
        else if(p->val > rootVal && q->val < rootVal || p->val < rootVal && q->val > rootVal)
        {
            return root;
        }
        else if(p->val > rootVal && q->val > rootVal)
        {
            return lowestCommonAncestor(root->right, p, q);
        }
        else
        {
            return lowestCommonAncestor(root->left, p, q);
        }
    }
};
```

## 3. 递归改循环

发现上面的迭代过程可以改成循环. 可以将空间复杂度O(log n)降为O(1)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while(true)
        {
            if(root == nullptr)
                return root;
            int rootVal = root->val;
            if(p->val == rootVal || q->val == rootVal)
            {
                return root;
            }
            else if(p->val > rootVal && q->val < rootVal || p->val < rootVal && q->val > rootVal)
            {
                return root;
            }
            else if(p->val > rootVal && q->val > rootVal)
            {
                root = root->right;
            }
            else
            {
                root = root->left;
            }
        }
        
    }
};
```

