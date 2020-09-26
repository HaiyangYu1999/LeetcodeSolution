Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only *distinct* numbers from the original list.

Return the linked list sorted as well.

**Example 1:**

```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```

**Example 2:**

```
Input: 1->1->1->2->3
Output: 2->3
```

## 双指针

这题的思路很简单, 就是对于每一个元素value, 用双指针begin, end来界定value的范围, 如果这个范围大于1, 说明就有重复的. **就要删去这个范围的所有节点**.

例如, 链表 `head -> 1 -> 1 -> 1 -> 2 -> 3`

首先令begin = head, end = head, 当前的元素值currVal = begin->next->val = 1.

然后, 我们去寻找链表中最后一个currVal

```java
int currVal = begin.next.val;
while(end.next != null && end.next.val == currVal)
{
	end = end.next;
}
```

此时退出循环后, end指向第3个1. 因为`end != begin.next` 说明元素1肯定是重复的, 所以就删除所有的1, `begin.next = end.next;`. 链表变为`head -> 2 -> 3`

然后进入第二次外部的while循环, 当前的元素值currVal = begin->next->val = 2.

寻找最后一个currVal, 因为只有1个2, 所以最后一个currVal的指针`end`等于`begin.next`, 所以元素2没有重复, 不用删除. 将begin前进一步进入下一次外部的while循环即可.

以此类推, 直到`begin.next == null` (我们在循环中控制着当`end.next == null`时就停止继续向下查找相同的元素, 所以不用担心`begin == null` 或 `end = null`这种事情的发生)

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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummyHead = new ListNode(-1, head);
        ListNode begin = dummyHead;
        ListNode end = dummyHead;
        while(begin.next != null)
        {
            int currVal = begin.next.val;
            while(end.next != null && end.next.val == currVal)
            {
                end = end.next;
            }
            if(end != begin.next)
                begin.next = end.next;
            else
            {
                begin = end;
            }
        }
        return dummyHead.next;
    }
}
```

