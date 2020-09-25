Given a linked list, remove the *n*-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given *n* will always be valid.

**Follow up:**

Could you do this in one pass?

## 快慢指针

因为是删除倒数第n个点. 我们可以设置2个指针, 快指针比慢指针快n个节点. 所以, 当我们再次遍历快慢指针时(快慢指针每一次都走1步), **当快指针走到链表尾部的时候, 慢指针正好走到倒数第n个节点的前一个节点**, 记为prev. 我们只需要令`prev->next = prev->next->next`即可删去倒数第n个节点.

Tips, 这个题给的链表是不带头结点的. 为了处理特殊情况, 比如链表只有1个元素要删除倒数第1个元素, 假如用上述方法会产生异常. 所以可以先添加链表头节点`ListNode* listHead = new ListNode(0, head);`, 再进行操作.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* listHead = new ListNode(0, head);
        ListNode* fast = listHead;
        ListNode* slow = listHead;
        while(n--)
            fast = fast->next;
        while(fast->next)
        {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return listHead->next;
    }
};
```

