Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

 

**Example 1:**

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Example 2:**

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Example 3:**

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

## 快慢指针

两个指针slow和fast, slow一次前进一步, fast前进2步, 判断在碰到nullptr前两个指针是否相等即可

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == nullptr)
            return false;
        ListNode* slow = head;
        ListNode* fast = head;
        bool hasCycle = false;
        while(true)
        {
            if(slow->next == nullptr)
                break;
            slow = slow->next;
            if(fast->next == nullptr || fast->next->next == nullptr)
                break;
            fast = fast->next->next;
            if(fast == slow)
            {
                hasCycle = true;
                break;
            }
        }
        return hasCycle;
    }
};
```

