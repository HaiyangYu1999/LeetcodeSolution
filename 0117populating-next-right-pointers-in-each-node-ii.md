Given a binary tree

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

![img](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

```
Input: root = [1,2,3,4,5,null,7]
Output: [1,#,2,3,#,4,5,7,#]
Explanation: Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

 

**Constraints:**

- The number of nodes in the given tree is less than `6000`.
- `-100 <= node.val <= 100`

## 1 队列

实在想不出递归的方法了. 主要是下面这样的树没法递归地处理. 因为没法找到左子树的最右侧元素和右子树的最左侧元素. 

只能用队列分层处理了

```
       1
     /   \
    2     3
   / \   / \
  4   5 6   7
 /           \
8             9
```

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
        Queue<Node> q = new ArrayDeque<>();
        q.offer(root);
        while(!q.isEmpty())
        {
            int size = q.size();
            Node beginNode = new Node();
            while(size > 0)
            {
                Node nextNode = q.poll();
                --size;
                beginNode.next = nextNode;
                beginNode = nextNode;
                if(beginNode.left != null)
                    q.offer(beginNode.left);
                if(beginNode.right != null)
                    q.offer(beginNode.right);
            }
        }
        return root;
    }
}
```

## 2 链表

这个想法真的好. 

我们利用队列的目的就是储存上一层的节点, 以方便把这一层所有节点都加入到队列中以便连接.

但是事实上, 获得上一层节点不需要一个队列来存储. **因为上一层节点已经是连接好的了, 所以只需要传递一个上一层所有元素的首指针`preLevel`即可获得这一层所有元素**, 然后连接他们, 再继续传递到下一层.

真的是妙啊.

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
        Node prevLevel = root;
        while(prevLevel != null)
        {
            Node currHead = new Node();
            Node tail = currHead;
            Node prevHead = prevLevel;
            while(prevHead != null)
            {
                if(prevHead.left != null)
                {
                    tail.next = prevHead.left;
                    tail = tail.next;
                }
                if(prevHead.right != null)
                {
                    tail.next = prevHead.right;
                    tail = tail.next;
                }
                prevHead = prevHead.next;
            }
            prevLevel = currHead.next;
        }
        return root;
    }
}
```

