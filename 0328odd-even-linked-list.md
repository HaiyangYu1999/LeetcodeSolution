Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

**Example 1:**

```
Input: 1->2->3->4->5->NULL
Output: 1->3->5->2->4->NULL
```

**Example 2:**

```
Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL
```

 

**Constraints:**

- The relative order inside both the even and odd groups should remain as it was in the input.
- The first node is considered odd, the second node even and so on ...
- The length of the linked list is between `[0, 10^4]`.

## 链表操作

不知道这题为什么定位为medium. 直接把偶数位置的节点截取下来组成一个新的链表然后接在原链表后面就可以了嘛

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
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        ListNode evenHead = new ListNode();
        ListNode tail = evenHead;
        ListNode prev = null;
        for(ListNode i = head; i != null; i = i.next)
        {
            if(i.next == null)
                break;
            ListNode even = i.next;
            i.next = i.next.next;
            even.next = null;
            tail.next = even;
            tail = tail.next;
            prev = i;
        }
        if(prev.next != null)
        {
            prev = prev.next;
        }
        prev.next = evenHead.next;
        return head;
    }
}
```

