Remove all elements from a linked list of integers that have value ***val\***.

**Example:**

```
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
```

## 模拟

直接做即可, 注意添上头节点更简单. 还要注意两个val相邻的情况. 如果进行了删除操作`i.next = i.next.next`,就不要`i = i.next`了. 否则会错

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
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummyHead = new ListNode(0, head);
        ListNode i = dummyHead;
        while(i != null)
        {
            if(i.next == null)
                break;
            if(i.next.val == val)
            {
                i.next = i.next.next;
            }
            else
            {
                i = i.next;
            }
        }
        return dummyHead.next;
    }
}
 ```

