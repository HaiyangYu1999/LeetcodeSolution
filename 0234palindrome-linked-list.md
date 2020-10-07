Given a singly linked list, determine if it is a palindrome.

**Example 1:**

```
Input: 1->2
Output: false
```

**Example 2:**

```
Input: 1->2->2->1
Output: true
```

**Follow up:**
Could you do it in O(n) time and O(1) space?

## 链表反转

首先通过快慢指针, 找到链表的中间节点middle, 然后翻转middle之后的元素. 之后, 从head和middle处逐个开始比较元素.

不知道为什么只超过了5%的时间. 

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
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null)
            return true;
        ListNode slow = head;
        ListNode fast = head;
        while(true)
        {
            if(fast.next == null || fast.next.next == null)
                break;
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode middle = slow;
        //reverse the second half part of the linked list
        ListNode p1 = middle.next;
        ListNode p2 = middle.next.next;
        while(p2 != null)
        {
            ListNode tmp = p2.next;
            p2.next = p1;
            p1 = p2;
            p2 = tmp;
        }
        middle.next.next = null;
        middle.next = p1;
        middle = middle.next;
        ListNode head1 = head;
        for(ListNode i = head; i != null; i = i.next)
            System.out.print(i.val + " ");
        while(middle != null)
        {
            if(head1.val != middle.val)
                return false;
            head1 = head1.next;
            middle = middle.next;
        }
        return true;
    }
}
```

