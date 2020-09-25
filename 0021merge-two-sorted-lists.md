Merge two sorted linked lists and return it as a new **sorted** list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

## 归并

和归并排序方法一样, 2个链表就创立2个指针. 比较哪个指针指向的元素小再插入, 同时该指针要向后移一位.

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* listHead = new ListNode();
        ListNode* tail = listHead;
        ListNode* p1 = l1;
        ListNode* p2 = l2;
        while(p1 != nullptr || p2!= nullptr)
        {
            if(p1 == nullptr)
            {
                tail->next = new ListNode(p2->val);
                p2 = p2->next;
                tail = tail->next;
            }
            else if(p2 == nullptr)
            {
                tail->next = new ListNode(p1->val);
                p1 = p1->next;
                tail = tail->next;
            }
            else
            {
                if(p1->val > p2->val)
                {
                    tail->next = new ListNode(p2->val);
                    p2 = p2->next;
                    tail = tail->next;
                }
                else
                {
                    tail->next = new ListNode(p1->val);
                    p1 = p1->next;
                    tail = tail->next;
                }
            }
        }
        return listHead->next;
    }
};
```

