You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## 常规做法

设置变量isOverflow判断是否要进位

先计算两个list的个位数和, `x = l1->val, y = l2->val`加起来, `tmp = x + y`

+ 若tmp > 9 说明要进位, isOverflow设为true, 并new一个新的ListNode(tmp)

+ 否则, 直接new新的ListNode(tmp)

最后, 更新x, y的值, 更新ListNode(tmp)将要插入的位置

在计算十位数的和, `tmp = x + y + ?` 其中若前面产生进位, `?`为1, 否则为0

以此类推直至两个list遍历到nullptr,并且isOverflow为false

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* i = l1;
        ListNode* j = l2;
        ListNode* ans = new ListNode(-1); //let -1 indicate the head node
        ListNode* tail = ans;
        bool isOverflow = false;
        while(i != nullptr || j != nullptr || isOverflow)
        {
            int x = (i == nullptr) ? 0 : i->val;
            int y = (j == nullptr) ? 0 : j->val;
            int tmp = x + y + (isOverflow ? 1 : 0);
            if(tmp > 9)
            {
                tmp -= 10;
                isOverflow = true;
            }
            else
            {
                isOverflow = false;
            }
            tail->next = new ListNode(tmp);
            tail = tail -> next;
            if(i != nullptr)
                i = i->next;
            if(j != nullptr)
                j = j->next;
        }
        if(ans -> next != nullptr)
            ans = ans->next; //delete the head node
        return ans;
    }
};
```

