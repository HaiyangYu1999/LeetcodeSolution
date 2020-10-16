You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

 

**Follow up:**

- You may only use constant extra space.
- Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

 

**Constraints:**

- The number of nodes in the given tree is less than `4096`.
- `-1000 <= node.val <= 1000`

## 递归

看到这个题之后的第一反应就是按层处理, 把每一层的所有节点都放到一个队列中, 然后挨个把他们连接起来. 后来发现题目要求不能使用除递归外的额外空间. 所以只能用递归做了

后来想了一会, 发现递归好像比队列更加简单.

首先递归边界是null或者叶子节点.

然后connect(root)就等价于先connect两个子树, 然后**再把左子树最右侧的节点与右子树最左侧的节点连接起来**

左子树a的所有最右侧节点定义为`a, a.right, a.right.right, ....`

右子树b的所有最左侧节点定义为`b, b.left, b.left.left, ....`

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        if(root == null || root.left == null && root.right == null)
            return root;
        else
        {
            connect(root.left);
            connect(root.right);
            Node left = root.left;
            Node right = root.right;
            while(left != null)
            {
                left.next = right;
                left = left.right;
                right = right.left;
            }
        }
        return root;
    }
}
```

