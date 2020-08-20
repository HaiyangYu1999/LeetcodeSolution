Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

**Note:** Do not modify the linked list.

 

**Example 1:**

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Example 2:**

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Example 3:**

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

 

**Follow-up**:
Can you solve it without using extra space?

## 1 暴力

建立一个ListNode* 到 int 的映射, 存储每个节点的访问次数.

依次访问链表, 每一次访问就增加1次访问次数, 返回第一个使访问次数为2的节点.

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_map<ListNode*, int> mp;
        while(head)
        {
            ++mp[head];
            if(mp[head] == 2)
                return head;
            head = head->next;
        }
        return nullptr;
    }
};
```

## 2 快慢指针

首先仍然是构造快慢指针fast, slow, 快指针一次走2步, 慢指针一次走1步. 直到他们相遇.

关键点来了

**两个指针在某个节点相遇后, 将慢指针移至头节点, 快指针不变, 然后快慢指针一次都走1步, 再次相遇的点即为环开始的点!!**

证明如下:

记链表中的环的长度为b, 其余长度为a, 两个指针第一次相遇是在慢指针进入环内x步后相遇的.

那么慢指针从开始到相遇一共走了 `s = a + x`步, 快指针自然是走了`f = 2a + 2x`步. 同时, 快指针走的所有路程为开始的a个节点, 在环中转了`n`次圈, 在加上最后的`x`个节点. 所以`f = a + x +nb`

上面三个式子结合, 能推导出`a + x = nb`

下面快指针变为走1步了, 慢指针从头节点开始, 若想要使得慢指针停留在环的第一个位置, 则慢指针必须走k步,k满足 `k = a + mb`, 其中m是正整数. 当快指针走k步的时候, 总步数`f=2a + 2x + a +mb`, 由上面的推导, a+x是b的倍数, 所以f = a (mod b), 即慢指针走a + mb步后快指针也会在环的第一个位置. 

取m = 0, 即变为寻找快慢指针第一个相遇的位置. 

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
    ListNode *detectCycle(ListNode *head) {
        if(head == nullptr)
            return nullptr;
        ListNode* fast = head;
        ListNode* slow = head;
        do
        {
            if(fast->next && fast->next->next)
                fast = fast->next->next;
            else
                return nullptr;
            if(slow->next)
                slow = slow->next;
            else
                return nullptr;
        }while(fast != slow);
        slow = head;
        while(fast != slow)
        {
            fast = fast->next;
            slow = slow->next;
        }
        return fast;
    }
};
```

