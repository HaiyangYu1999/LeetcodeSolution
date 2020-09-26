Given a linked list, swap every two adjacent nodes and return its head.

You may **not** modify the values in the list's nodes, only nodes itself may be changed.

 

**Example:**

```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

## 三指针

首先照常是为了方便加上头节点.

对于第2k个节点和第2k+1个节点, 要交换他们, 需要三个节点的指针. before(2k-1), curr(2k), after(2k+1).

然后执行下面的命令达到交换的目的. 

```java
ListNode tmp = after.next;
before.next = after;
after.next = curr;
curr.next = tmp;
```

指针交换完之后要判断是不是要继续往下交换. 我们用tmp保存了交换前的after.next变量. 如果tmp != null 并且 tmp.next != null 就说明链表后面至少还有2个节点, 要继续交换, 要更新before, curr, after 的值

```java
before = curr;
curr = tmp;
after = tmp.next;
```

在纸上画链表图之后很容易看出这两个代码块的操作是怎么回事了.

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
    public ListNode swapPairs(ListNode head) {
        ListNode headList = new ListNode(500, head);
        ListNode before = headList;
        ListNode curr = headList.next;
        if(curr == null || curr.next == null)
            return head;
        ListNode after = curr.next;
        while(true)
        {
            ListNode tmp = after.next;
            before.next = after;
            after.next = curr;
            curr.next = tmp;
            if(tmp == null || tmp.next == null)
                break;
            before = curr;
            curr = tmp;
            after = tmp.next;
        }
        return headList.next;
    }
}
```

