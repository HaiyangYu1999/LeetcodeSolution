Write a function to **delete a node** in a singly-linked list. You will **not** be given access to the `head` of the list, instead you will be given access to **the node to be deleted** directly.

It is **guaranteed** that the node to be deleted is **not a tail node** in the list.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/01/node1.jpg)

```
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/09/01/node2.jpg)

```
Input: head = [4,5,1,9], node = 1
Output: [4,5,9]
Explanation: You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function.
```

**Example 3:**

```
Input: head = [1,2,3,4], node = 3
Output: [1,2,4]
```

**Example 4:**

```
Input: head = [0,1], node = 0
Output: [1]
```

**Example 5:**

```
Input: head = [-3,5,-99], node = -3
Output: [5,-99]
```

 

**Constraints:**

- The number of the nodes in the given list is in the range `[2, 1000]`.
- `-1000 <= Node.val <= 1000`
- The value of each node in the list is **unique**.
- The `node` to be deleted is **in the list** and is **not a tail** node

## 交换节点的值

一开始想这个题总想不出来, 感觉必须要获得这个节点前面的一个节点才能删除.

后来看了答案才发现只需要交换值就可以!!!

**把这个节点的后一个节点的值覆盖到这个节点上, 然后删除后一个节点就可以了!!**

可能是之前做链表题, 有很多链表题都要求只能进行指针的计算不能交换值这个条件限制了思路.

还是应该打开思路, 多多创新.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public void deleteNode(ListNode node) {
        if(node.next.next == null)
        {
            node.val = node.next.val;
            node.next = null;
        }
        else
        {
            node.val = node.next.val;
            node.next = node.next.next;
        }
    }
}
```

