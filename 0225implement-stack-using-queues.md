Implement a last in first out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal queue (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a queue, which means only `push to back`, `peek/pop from front`, `size`, and `is empty` operations are valid.
- Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue), as long as you use only a queue's standard operations.

**Follow-up:** Can you implement the stack such that each operation is **[amortized](https://en.wikipedia.org/wiki/Amortized_analysis)** `O(1)` time complexity? In other words, performing `n` operations will take overall `O(n)` time even if one of those operations may take longer.

 

**Example 1:**

```
Input
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 2, 2, false]

Explanation
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // return 2
myStack.pop(); // return 2
myStack.empty(); // return False
```

 

**Constraints:**

- `1 <= x <= 9`
- At most `100` calls will be made to `push`, `pop`, `top`, and `empty`.
- All the calls to `pop` and `top` are valid.

## 1 2个队列. push O(1), pop O(n)

2个队列互相交替, 插入的时候维持原有顺序, pop的时候把含有element的队列的前n-1个元素转移到另外一个队列, 第n个元素pop出来.

```java
class MyStack {
    Queue<Integer> a;
    Queue<Integer> b;
    /** Initialize your data structure here. */
    public MyStack() {
        a = new ArrayDeque<>();
        b = new ArrayDeque<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        Queue<Integer> full;
        Queue<Integer> empty;
        if(!a.isEmpty())
        {
            full = a;
            empty = b;
        }
        else
        {
            full = b;
            empty = a;
        }
        full.offer(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        Queue<Integer> full;
        Queue<Integer> empty;
        if(!a.isEmpty())
        {
            full = a;
            empty = b;
        }
        else
        {
            full = b;
            empty = a;
        }
        while(full.size() > 1)
        {
            int tmp = full.poll();
            empty.offer(tmp);
        }
        return full.poll();
    }
    
    /** Get the top element. */
    public int top() {
        Queue<Integer> full;
        Queue<Integer> empty;
        if(!a.isEmpty())
        {
            full = a;
            empty = b;
        }
        else
        {
            full = b;
            empty = a;
        }
        while(full.size() > 1)
        {
            int tmp = full.poll();
            empty.offer(tmp);
        }
        int ans = full.peek();
        empty.offer(full.poll());
        return ans;
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return a.isEmpty() && b.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

## 2 2个队列. push O(n), pop O(1)

插入的时候就把顺序导致过来, 通过两个队列的交替始终保持最新插入的元素在队列第一个位置. pop和top只需要O(1)的操作

感觉这一种方法比第一种好一些.

```java
class MyStack {
    Queue<Integer> a;
    Queue<Integer> b;
    /** Initialize your data structure here. */
    public MyStack() {
        a = new ArrayDeque<>();
        b = new ArrayDeque<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        Queue<Integer> full;
        Queue<Integer> empty;
        if(!a.isEmpty())
        {
            full = a;
            empty = b;
        }
        else
        {
            full = b;
            empty = a;
        }
        empty.offer(x);
        while(!full.isEmpty())
        {
            empty.offer(full.poll());
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        Queue<Integer> full;
        Queue<Integer> empty;
        if(!a.isEmpty())
        {
            full = a;
            empty = b;
        }
        else
        {
            full = b;
            empty = a;
        }
        return full.poll();
    }
    
    /** Get the top element. */
    public int top() {
        Queue<Integer> full;
        Queue<Integer> empty;
        if(!a.isEmpty())
        {
            full = a;
            empty = b;
        }
        else
        {
            full = b;
            empty = a;
        }
        return full.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return a.isEmpty() && b.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

## 3 1个队列

这种方法真是妙啊.

还是插入的时候就要颠倒次序.

假如已经插入了1,2,3,4.

这时队列为 4,3,2,1.

当插入5的时候, 先记录队列的大小size.

然后插入5, 队列变为4,3,2,1,5

**然后队列首元素出队列再进队列size次**

即4,3,2,1,5 -> 3,2,1,5,4 -> 2,1,5,4,3 -> 1,5,4,3,2 -> 5,4,3,2,1

这样就完成了队列到栈的转换.

```java
class MyStack {
    Queue<Integer> a;
    /** Initialize your data structure here. */
    public MyStack() {
        a = new ArrayDeque<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        int size = a.size();
        a.offer(x);
        while(size > 0)
        {
            a.offer(a.poll());
            --size;
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return a.poll();
    }
    
    /** Get the top element. */
    public int top() {
        return a.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return a.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

