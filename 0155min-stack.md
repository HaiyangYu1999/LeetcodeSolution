Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

 

**Example 1:**

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

 

**Constraints:**

- Methods `pop`, `top` and `getMin` operations will always be called on **non-empty** stacks.

## 1 辅助栈

这题一开始想了很长时间, 思路被限制在哈希表和优先队列上面了. 实际上用两个栈就ok了

第一个栈就存储各个元素x1, x2, x3,...,xn, 第二个栈就存储M1, M2, M3,...., Mn. 其中Mi为x1到xi的最小值.

然后就很容易进行操作了.

```java
class MinStack {

    Deque<Integer> stk;
    Deque<Integer> stkAux;
    
    /** initialize your data structure here. */
    public MinStack() {
        stk = new ArrayDeque<>();
        stkAux = new ArrayDeque<>();
        stkAux.offerFirst(Integer.MAX_VALUE);
    }
    
    public void push(int x) {
        stk.offerFirst(x);
        stkAux.offerFirst(Math.min(x,stkAux.peekFirst()));
    }
    
    public void pop() {
        stk.pollFirst();
        stkAux.pollFirst();
    }
    
    public int top() {
        return stk.peekFirst();
    }
    
    public int getMin() {
        return stkAux.peekFirst();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

## 2 O(1)空间复杂度

这次要改变一下思路, stack不储存各个元素了, 而是存储各个元素与min的差值.

参考自https://zhuanlan.zhihu.com/p/49854919, 具体如下: 

1. 第一次push的时候，把该元素作为最小元素min。
2. 在后面的push操作中，首先判断当前元素`num`是否小于`min`,如果不小于`min`，就向栈中存入元素值`data = num-min`(这个值肯定大于0，因为num大于min)；如果num小于min，也向栈中存入`data = num-min`(`data`小于0)，同时记得更新`min`值。
3. pop的时候，首先判断栈顶的元素data是否大于0，如果大于0，则pop的值应该是`num=data +min`（因为存的时候是`data = num-min`);如果小于0，则pop的时候应该是`min`，同时要更新min，`min = min- data`。
4. 同时get_min时直接返回min的值就是整个栈元素中的最小值

```java
class MinStack {

    Deque<Long> stk;
    long min;
    
    /** initialize your data structure here. */
    public MinStack() {
        stk = new ArrayDeque<>();
    }
    
    public void push(int x) {
        if(stk.isEmpty())
        {
            min = x;
            stk.offerFirst((long) 0);
        }
        else
        {
            stk.offerFirst(x - min);
            if(x < min)
                min = x;
        }
    }
    
    public void pop() {
        if(stk.peekFirst() < 0)
        {
            min = min - stk.peekFirst();
            stk.pollFirst();
        }
        else
        {
            stk.pollFirst();
        }
    }
    
    public int top() {
        return (stk.peekFirst() < 0) ? (int) min : (int) (stk.peekFirst() +  min);
    }
    
    public int getMin() {
        return (int) min;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

