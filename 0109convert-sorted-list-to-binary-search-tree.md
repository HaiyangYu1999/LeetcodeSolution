Given the `head` of a singly linked list where elements are **sorted in ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

```
Input: head = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.
```

**Example 2:**

```
Input: head = []
Output: []
```

**Example 3:**

```
Input: head = [0]
Output: [0]
```

**Example 4:**

```
Input: head = [1,3]
Output: [3,1]
```

 

**Constraints:**

- The number of nodes in `head` is in the range `[0, 2 * 104]`.
- `-10^5 <= Node.val <= 10^5`

## 1 递归

和leetcode 108 差不多.

只不过这里找到中间元素slow的同时, 还要找到slow的前面的元素, 以断裂链表. 将链表分为两个子链表

分治, 时间复杂度O(nlogn)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
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
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null)
            return null;
        if(head.next == null)
            return new TreeNode(head.val);
        ListNode fast = head;
        ListNode slow = head;
        ListNode midPrev = head;
        while(fast != null && fast.next != null)
        {
            fast = fast.next.next;
            midPrev = slow;
            slow = slow.next;
        }
        TreeNode ans = new TreeNode(slow.val);
        midPrev.next = null;
        ans.left = sortedListToBST(head);
        ans.right = sortedListToBST(slow.next);
        return ans;
    }
}
```

## 2 转化为数组

为了优化时间复杂度, 可以把链表转化为数组, 然后和leetcode 108 一样. 时间复杂度O(n)

## 3 利用中序遍历

https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/solution/you-xu-lian-biao-zhuan-huan-er-cha-sou-suo-shu-1-3/

这是一种很巧妙的方法, 先确定开始和结束的位置 begin 和 end, 然后逐次往里面添加链表中的值.

利用globalHead控制链表的遍历, 用begin和end控制二叉树的结构, 即哪些部分是左子树的, 哪些部分是右子树的. 十分巧妙. 时间复杂度O(n)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
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
    private ListNode globalHead;
    public TreeNode sortedListToBST(ListNode head) {
        globalHead = head;
        int len = getLength(head);
        return buildTree(0, len - 1);
    }
    private TreeNode buildTree(int begin, int end)
    {
        if(begin > end)
            return null;
        int mid = begin + (end - begin) / 2;
        TreeNode ans = new TreeNode();
        ans.left = buildTree(begin, mid - 1);
        ans.val = globalHead.val;
        globalHead = globalHead.next;
        ans.right = buildTree(mid + 1, end);
        return ans;
    }
    private int getLength(ListNode head)
    {
        int len = 0;
        while(head != null)
        {
            ++len;
            head = head.next;
        }   
        return len;
    }
}
```

