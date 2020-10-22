Sort a linked list using insertion sort.



![img](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)
A graphical example of insertion sort. The partial sorted list (black) initially contains only the first element in the list.
With each iteration one element (red) is removed from the input data and inserted in-place into the sorted list
 



**Algorithm of Insertion Sort:**

1. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
3. It repeats until no input elements remain.


**Example 1:**

```
Input: 4->2->1->3
Output: 1->2->3->4
```

**Example 2:**

```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```

## 插入排序

首先列表在中间断开, 排序的是一个链表sortedListHead, 未排序的是另外的链表remainderListHead

当然, 最初的时候sortedListHead只有1个元素

> ```
> -1->5->3->4->0  =>    -1    5->3->4->0
> ```

然后只要remainderListHead非空, 就在里面取出首元素curr来, 

> ```
> -1    5->3->4->0  ==> -1   5   3->4->0
> ```

然后将curr归置到sortedListHead相应的位置.

> ```
> -1   5   3->4->0   ==> -1->5  3->4->0
> ```

以此类推,

> ```
> -1->5  3->4->0  ==> -1->5  3  4->0
> -1->5  3  4->0  ==> -1->3->5  4->0
> ```

知道remainderListHead为空

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
    public ListNode insertionSortList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        ListNode sortedListHead = head;
        ListNode remainderListHead = head.next;
        head.next = null;
        while(remainderListHead != null)
        {
            ListNode curr = remainderListHead;
            remainderListHead = remainderListHead.next;
            curr.next = null;
            //now insert Node curr into sortedListHead.
            if(curr.val < sortedListHead.val)
            {
                curr.next = sortedListHead;
                sortedListHead = curr;
                continue;
            }
            else
            {
                ListNode prev = sortedListHead;
                ListNode next_ = sortedListHead.next;
                while(next_ != null && next_.val < curr.val)
                {
                    next_ = next_.next;
                    prev = prev.next;
                }
                // now we have prev < curr < next_ or prev < curr < null
                prev.next = curr;
                curr.next = next_;
            }
        }
        return sortedListHead;
    }
}
```

