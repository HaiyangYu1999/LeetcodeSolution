Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:

[![img](https://assets.leetcode.com/uploads/2018/12/13/160_statement.png)](https://assets.leetcode.com/uploads/2018/12/13/160_statement.png)

begin to intersect at node c1.

 

**Example 1:**

[![img](https://assets.leetcode.com/uploads/2020/06/29/160_example_1_1.png)](https://assets.leetcode.com/uploads/2020/06/29/160_example_1_1.png)

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Reference of the node with value = 8
Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```

 

**Example 2:**

[![img](https://assets.leetcode.com/uploads/2020/06/29/160_example_2.png)](https://assets.leetcode.com/uploads/2020/06/29/160_example_2.png)

```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Reference of the node with value = 2
Input Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```

 

**Example 3:**

[![img](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: null
Input Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

 

**Notes:**

- If the two linked lists have no intersection at all, return `null`.
- The linked lists must retain their original structure after the function returns.
- You may assume there are no cycles anywhere in the entire linked structure.
- Each value on each linked list is in the range `[1, 10^9]`.
- Your code should preferably run in O(n) time and use only O(1) memory.

## 1 求链表长度

先求出两个链表的长度, len1和len2, 这里假设len1大. diff = abs(len1 - len2).

然后指向list1的指针先走diff步, 然后指向list1的指针和list2的指针再一起走. 当某一步两个指针指向的元素相同时, 就返回. 当某个指针指向了null, 就返回null.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null)
            return null;
        int len1 = 0;
        int len2 = 0;
        ListNode tmp1 = headA;
        ListNode tmp2 = headB;
        while(tmp1 != null)
        {
            ++len1;
            tmp1 = tmp1.next;
        }
        while(tmp2 != null)
        {
            ++len2;
            tmp2 = tmp2.next;
        }
        boolean isListALonger = (len1 >= len2);
        int diff = Math.abs(len1 - len2);
        ListNode p1 = headA;
        ListNode p2 = headB;
        if(isListALonger)
        {
            while(diff != 0)
            {
                p1 = p1.next;
                --diff;
            }
        }
        else
        {
            while(diff != 0)
            {
                p2 = p2.next;
                --diff;
            }
        }
        while(true)
        {
            if(p1 == null || p2 == null)
                return null;
            if(p1 == p2)
                return p1;
            p1 = p1.next;
            p2 = p2.next;
        }
        
    }
}
```

## 2 双指针

这是一种更巧妙的方法. 

指针a和指针b同时走, 指针a走到null时就指向链表b的开头, 指针b走到null时就指向链表a的开头. 最后, 如果他们在某一点相遇(之前都没相遇), 这个点肯定是链表相交的点!

一开始可能难以理解, 但是我们假设, 链表a和链表b的不相交部分的长度为a和b, 公共部分长度为c. 并且假设链表a长

当两个指针开始走的时候, 首先指针b先走完, 走了b+c步. 然后转移到链表a的开头. 然后两个指针再继续走, 指针a走完, 走了a+c步, 指向b的开头. 当a再走完b步的时候, a和b都走了a+c+b步. 正好处于相交处(可以画图, 更容易理解).

但是感觉比方法1并没有快多少.... 复杂度都是O(m+n), 只是代码少, 但是难以想到

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null)
            return null;
        ListNode a = headA;
        ListNode b = headB;
        while(true)
        {
            if(a == b)
                return a;
            a = (a == null) ? headB : a.next;
            b = (b == null) ? headA : b.next;
        }
    }
}
```

