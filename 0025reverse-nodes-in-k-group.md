Given a linked list, reverse the nodes of a linked list *k* at a time and return its modified list.

*k* is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of *k* then left-out nodes in the end should remain as it is.



**Example:**

Given this linked list: `1->2->3->4->5`

For *k* = 2, you should return: `2->1->4->3->5`

For *k* = 3, you should return: `3->2->1->4->5`

**Note:**

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes, only nodes itself may be changed.

## 构造辅助函数(reverseFrom)

这个题看上去和leetcode 24 很像, 只不过把reverse 2换成了reverse k. 实际上不然. 这题和 leetcode 92更像.

在leetcode 92中, 我们reverse了从第m到第n个节点. 在这个题中, 只要求出了链表长度len, 那么就需要执行cnt = len/k次reverse.

每一次reverse都是reverse第`i*k`个到第`i*k + k-1`个节点. 

所以构造函数`ListNode reverseFrom(ListNode prev, int k)`

这个函数交换从prev.next开始之后的k个节点(从prev.next + 0到prev.next + k-1), 并返回被reverse的第k个节点ret. 作为下一次函数调用的输入.

例如 head->1 -> 2 ->3 -> 4,

我们调用完 `reverseFrom(head, 2)`之后, 链表变为head->2 -> **1** ->3 -> 4, 返回值ret指向新链表中的1. 那么当我们再调用`reverseFrom(ret, 2)`后, 链表就变为了head->2 -> 1 ->4 -> **3**. 此时ret指向3

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
    public ListNode reverseKGroup(ListNode head, int k) {
        if(k <= 1)
            return head;
        int length = 0;
        ListNode dummyHead = new ListNode(-1, head);
        while(head != null)
        {
            ++length;
            head = head.next;
        }
        int cnt = length / k;
        ListNode prev = dummyHead;
        for(int i = 0; i < cnt; ++i)
        {
            prev = reverseFrom(prev, k);
        }
        return dummyHead.next;
    }
    
    private ListNode reverseFrom(ListNode prev, int k) {
        ListNode p1 = prev.next;
        ListNode p2 = p1.next;
        for(int i = 0; i < k - 1; ++i)
        {
            ListNode tmp = p2.next;
            p2.next = p1;
            p1 = p2;
            p2 = tmp;
        }
        prev.next.next = p2;
        ListNode ret = prev.next;
        prev.next = p1;
        return ret;
    }
}
```

