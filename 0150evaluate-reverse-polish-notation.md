Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

**Note:**

- Division between two integers should truncate toward zero.
- The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example 1:**

```
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**

```
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3:**

```
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

## Stack

感觉是很基础的题了, 难度设为medium有点不合适. 学数据结构的时候肯定学过这个题!!

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stk = new ArrayDeque<>();
        for(String token : tokens)
        {
            if(!isSymbol(token))
            {
                stk.offerFirst(Integer.parseInt(token));
            }
            else
            {
                int b = stk.pollFirst();
                int a = stk.pollFirst();
                int res;
                if("+".equals(token))
                    res = a + b;
                else if("-".equals(token))
                    res = a - b;
                else if("*".equals(token))
                    res = a * b;
                else
                    res = a / b;
                stk.offerFirst(res);
            }
        }
        return stk.pollFirst();
    }
    
    private boolean isSymbol(String i)
    {
        return "+".equals(i) || "-".equals(i) || "*".equals(i) || "/".equals(i);
    }
}
```

