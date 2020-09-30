Reverse a linked list from position *m* to *n*. Do it in one-pass.

**Note:** 1 ≤ *m* ≤ *n* ≤ length of list.

**Example:**

```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```

## 原地reverse

思路和leetcode 206 中的原地reverse相同.

首先要记录第m-1个节点, 代码中为`mPrev`. 

然后建立双指针, `p1 = mPrev.next; p2 = p1.next;`因为题目确保了1 ≤ *m* ≤ *n* ≤ length of list.所以也不用检查p1和p2是不是null了.

此时开始reverse, **reverse n - m 次**, 就能保证m到n的次序是reverse过的. 而其他的不变.

循环结束后, p2指向第n+1个节点, 也就是在reverse range 之后, 并且没有被reverse的第一个节点. p1指向原链表第n个节点, 但是因为reverse了, 应该指向新链表第m个节点. 新链表第n个节点应该是原链表第m个节点也就是`mPrev.next`. 所以将新链表第n个节点与n+1个节点连接起来`mPrev.next.next = p2;`, 并且将原来链表第m-1节点与新链表第m节点连接起来`mPrev.next = p1;`

最后返回第1个节点.

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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode dummyHead = new ListNode(-1, head);
        if(head == null || head.next == null)
            return head;
        else if(m >= n)
            return head;
        else
        {
            ListNode mPrev = dummyHead;
            for(int i = 0; i < m - 1; ++i)
            {
                mPrev = mPrev.next;
            }
            ListNode p1 = mPrev.next;
            ListNode p2 = p1.next;
            for(int i = 0; i < n-m; ++i)
            {
                ListNode tmp = p2.next;
                p2.next = p1;
                p1 = p2;
                p2 = tmp;
            }
            //ListNode tmp = p2.next;
            mPrev.next.next = p2;
            mPrev.next = p1;
        }
        return dummyHead.next;
    }
}
```

