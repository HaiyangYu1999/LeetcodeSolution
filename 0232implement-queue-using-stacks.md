Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

**Follow-up:** Can you implement the queue such that each operation is **[amortized](https://en.wikipedia.org/wiki/Amortized_analysis)** `O(1)` time complexity? In other words, performing `n` operations will take overall `O(n)` time even if one of those operations may take longer.

 

**Example 1:**

```
Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

 

**Constraints:**

- `1 <= x <= 9`
- At most `100` calls will be made to `push`, `pop`, `peek`, and `empty`.
- All the calls to `pop` and `peek` are valid.

## 1 栈模拟1

设立两个栈stk1和stk2, stk1储存倒置顺序的元素, stk2为空.

push的时候先把stk1所有元素转移到stk2, 然后把x放到stk1栈底, 然后再把stk2剩下的元素返回到stk1, 这样, stk1底部多了元素x, 并且其他元素位置不变.

> 初始状态                                 清空stk1                                       push(5)                                   剩下的元素返回stk1
>
> stk1 :  4,3,2,1                          null                                              5                                               5,4,3,2,1
>
> stk2 :  null                               1,2,3,4                                        1,2,3,4                                       null

**但是这种push操作需要O(n)复杂度**. 

```java
class MyQueue {
    Stack<Integer> stk1;
    Stack<Integer> stk2;
    /** Initialize your data structure here. */
    public MyQueue() {
        stk1 = new Stack<Integer>();
        stk2 = new Stack<Integer>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        while(!stk1.isEmpty())
        {
            stk2.push(stk1.pop());
        }
        stk1.push(x);
        while(!stk2.isEmpty())
        {
            stk1.push(stk2.pop());
        }
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        return stk1.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        return stk1.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stk1.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

## 2 栈模拟2

上面的方法每push一次都要有O(n)的计算量.

可以改进一下, 设置一个pushstack和popstack.

每次push都直接push到pushstack中. pop或top的时候再把pushtack中的元素全都转移过来. 由于栈的特性, 转移到popstack后自然是队列的顺序了.

这样, 所有操作的复杂度都可以降为O(1)

```java
class MyQueue {
    Stack<Integer> pushStk;
    Stack<Integer> popStk;
    /** Initialize your data structure here. */
    public MyQueue() {
        pushStk = new Stack<Integer>();
        popStk = new Stack<Integer>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        pushStk.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(!popStk.isEmpty())
        {
            return popStk.pop();
        }
        else
        {
            while(!pushStk.isEmpty())
            {
                popStk.push(pushStk.pop());
            }
            return popStk.pop();
        }
    }
    
    /** Get the front element. */
    public int peek() {
        if(!popStk.isEmpty())
        {
            return popStk.peek();
        }
        else
        {
            while(!pushStk.isEmpty())
            {
                popStk.push(pushStk.pop());
            }
            return popStk.peek();
        }
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return pushStk.isEmpty() && popStk.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

