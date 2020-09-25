Given a linked list, rotate the list to the right by *k* places, where *k* is non-negative.

**Example 1:**

```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

**Example 2:**

```
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```

## 快慢指针

首先要遍历一遍链表求出长度len,

然后更新k = k % len. 因为假如k是len的整数倍的话, 旋转k次等于回到了原链表.

**然后, 我们只需要将原链表的最后k个node分离出来当成一个新的链表, 整体地插入到原链表头部即可 . **(当然, 原链表的倒数k+1个节点的next域要置为null)

+ 先使用快慢指针, 求得倒数第k+1个节点.
+ 记录下倒数第k个节点的指针ans, 作为新链表的头部.
+ 然后将倒数第k+1个节点的next置为null
+ 最后将原链表插入到新链表的尾部

```c++
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
    public ListNode rotateRight(ListNode head, int k) {
        int len = 0;
        ListNode list = head;
        while(list != null)
        {
            ++len;
            list = list.next;
        }
        if(len == 0)
            return head;
        k = k % len;
        if(k == 0) 
            return head;
        ListNode listHead = new ListNode(0, head);
        ListNode fast = listHead;
        ListNode slow = listHead;
        for(int i = 0; i < k; ++i)
        {
            fast = fast.next;
        }
        while(fast.next != null)
        {
            fast = fast.next;
            slow = slow.next;
        }
        ListNode ans = slow.next;
        ListNode ansTail = ans;
        slow.next = null;
        while(ansTail.next != null)
        {
            ansTail = ansTail.next;
        }
        ansTail.next = head;
        return ans;
    }
}
```

