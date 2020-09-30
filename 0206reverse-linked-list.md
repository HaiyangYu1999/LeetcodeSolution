Reverse a singly linked list.

**Example:**

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

**Follow up:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

## 1 迭代

对于链表1,2,3,4,5

创建一个新的空链表dummyHead,

遍历到1的时候, 把1加入到空链表头部, 空链表为head ->1

遍历到2的时候, 把2加入到链表头部, 变为head -> 2 -> 1

以此类推....

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
    public ListNode reverseList(ListNode head) {
        ListNode dummyHead = new ListNode();
        for(ListNode i = head; i != null; i = i.next)
        {
            ListNode tail = dummyHead.next;
            dummyHead.next = new ListNode(i.val, tail);
        }
        return dummyHead.next;
    }
}
```

## 2 递归

参考于 https://blog.csdn.net/SoulOH/article/details/81062223

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
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        else
        {
            ListNode p = reverseList(head.next);
            ListNode tail = head.next;
            tail.next = head;
            head.next = null;
            return p;
        }
    }
    
}
```

## 3 原地迭代reverse

同样参考于 https://blog.csdn.net/SoulOH/article/details/81062223

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
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        ListNode p1 = head;
        ListNode p2 = head.next;
        while(true)
        {
            ListNode tmp = p2.next;
            p2.next = p1;
            if(tmp == null)
                break;
            p1 = p2;
            p2 = tmp;
        }
        head.next = null;
        return p2;
    }
    
}
```

