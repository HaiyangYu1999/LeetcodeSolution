Given a singly linked list *L*: *L*0→*L*1→…→*L**n*-1→*L*n,
reorder it to: *L*0→*L**n*→*L*1→*L**n*-1→*L*2→*L**n*-2→…

You may **not** modify the values in the list's nodes, only nodes itself may be changed.

**Example 1:**

```
Given 1->2->3->4, reorder it to 1->4->2->3.
```

**Example 2:**

```
Given 1->2->3->4->5, reorder it to 1->5->2->4->3.
```

## 翻转链表

**思路比较简单**

首先说思路, 对于一个链表1->2->3->4->5->6->7

先把他从中间分成两部分, 1->2->3->4 和 5->6->7

然后翻转后面的链表5->6->7 ==> 7->6->5

然后对于1->2->3->4 和 7->6->5这两个链表, 合并起来, 变为1->7->2->6->3->5->4

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
    public void reorderList(ListNode head) {
        if(head == null || head.next == null)
            return;
        ListNode fast = head;
        ListNode slow = head;
        while(fast.next != null && fast.next.next != null)
        {
            fast = fast.next.next;
            slow = slow.next;
        }
        //reverse the node after slow
        reverse(slow);
        ListNode list2 = slow.next;
        slow.next = null;
        ListNode list1 = head;
        ListNode i = list1;
        ListNode j = list2;
        while(j != null)
        {
            //insert j to i_behind
            ListNode tmpj = j;
            j = j.next;
            tmpj.next = null;
            ListNode tmpi = i.next;
            i.next = tmpj;
            tmpj.next = tmpi;
            i = tmpi;
        }
    }
    private void reverse(ListNode head)
    {
        if(head.next == null || head.next.next == null)
            return;
        ListNode p1 = head.next;
        ListNode p2 = head.next.next;
        while(p2 != null)
        {
            ListNode tmp = p2.next;
            p2.next = p1;
            p1 = p2;
            p2 = tmp;
        }
        head.next.next = null;
        head.next = p1;
    }
}
```

