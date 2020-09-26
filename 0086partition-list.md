Given a linked list and a value *x*, partition it such that all nodes less than *x* come before nodes greater than or equal to *x*.

You should preserve the original relative order of the nodes in each of the two partitions.

**Example:**

```
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
```

## 构造新链表

这题一开始想的是不用额外空间做, 后来发现做不了

允许使用额外空间就很简单了,

构造两个新的空链表, less储存小于x的所有节点, greater储存大于等于x的所有节点, 原链表遍历一遍, 把每个节点都加到对应的新链表里, 然后这两条链表连起来就ok了

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
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode less = new ListNode(-1);
        ListNode greater = new ListNode(-1);
        ListNode lessTail = less;
        ListNode greaterTail = greater;
        while(head != null)
        {
            if(head.val < x)
            {
                lessTail.next = new ListNode(head.val);
                lessTail = lessTail.next;
            }
            else
            {
                greaterTail.next = new ListNode(head.val);
                greaterTail = greaterTail.next;
            }
            head = head.next;
        }
        lessTail.next = greater.next;
        return less.next;
    }
}
```

