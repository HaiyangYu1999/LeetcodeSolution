You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it.*

 

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```

 

**Constraints:**

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` won't exceed `10^4`.

## 1 k指针

首先创建一个空链表result, 对于lists中的k个指针, 选出指向元素最小的指针minIndex(空指针跳过不参加比较), 然后将这个元素添加到链表result中. 然后指针minIndex指向下一个元素, 其他指针不动. 再从这k个指针指向的元素中找到最小的, 以此类推.....直到所有指针全部为null.

时间复杂度O(kn), k是lists数量, n是链表长度和.

**这个方法慢的原因就是, 每一次添加一个元素之后都要从k个指针中找到最小的那一个!**

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* listHead = new ListNode();
        ListNode* tail = listHead;
        while(!areAllNull(lists))
        {
            int minIndex = -1;
            int min = INT_MAX;
            for(int i = 0; i < lists.size(); ++i)
            {
                if(lists[i] == nullptr)
                    continue;
                if(lists[i]->val < min)
                {
                    minIndex = i;
                    min = lists[i]->val;
                }
            }
            tail->next = new ListNode(lists[minIndex]->val);
            lists[minIndex] = lists[minIndex]->next;
            tail = tail->next;
        }
        return listHead->next;
    }
private:
    bool areAllNull(const vector<ListNode*>& lists)
    {
        for(ListNode* list : lists)
        {
            if(list != nullptr)
                return false;
        }
        return true;
    }
};
```

## 2 优先队列

因为每一次都是从k个指针指向的元素里面选最小的, 所以可以用优先队列来优化, 构建一个大小为k的优先队列, 可以将复杂度变为O(nlogk).

首先, 把所有的非空指针加入到队列中, 然后pop一个元素最小的指针min, 插入到返回队列中, 如果min->next非空, 就再继续把min->next加入到优先队列中. 直到优先队列为空.

这里要吐槽一下c++的STL, 几乎所有的操作都比Java的Collection好用, 就是自定义优先队列的时候不方便. 要新写一个`struct`, 重载`()`运算符. 

```c++
bool operator()(const ListNode* a, const ListNode* b)
{
	return a->val > b->val;
}
```

这里的`return a->val > b->val`指的是, 如果函数返回值是true，则证明函数中第一个参数的优先级比第二个参数小，优先级小的就该放到后面去.

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* listHead = new ListNode();
        ListNode* tail = listHead;
        priority_queue<ListNode*, vector<ListNode*>, ListNodeCmp> pq;
        for(ListNode* list : lists)
        {
            if(list != nullptr)
                pq.push(list);
        }
        while(!pq.empty())
        {
            ListNode* min = pq.top();
            pq.pop();
            tail->next = new ListNode(min->val);
            tail = tail->next;
            if(min->next != nullptr)
                pq.push(min->next);
        }
        return listHead->next;
    }
private:
    struct ListNodeCmp
    {
        bool operator()(const ListNode* a, const ListNode* b)
        {
            return a->val > b->val;
        }
    };
};
```

