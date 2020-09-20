Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

 

**Example 1:**

```
Input: s = "()"
Output: true
```

**Example 2:**

```
Input: s = "()[]{}"
Output: true
```

**Example 3:**

```
Input: s = "(]"
Output: false
```

**Example 4:**

```
Input: s = "([)]"
Output: false
```

**Example 5:**

```
Input: s = "{[]}"
Output: true
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

## stack 

实际上就是用stack模拟括号. 思路简单, 但是细节多.

首先要判断是左括号还是右括号. 左括号直接push到栈里面, 右括号要先**检查栈是不是空的, 是空的直接返回false**. 不是空的检查栈顶元素和右括号是否匹配, 如果不匹配直接返回false. 匹配就pop.

遍历完string中每一个元素后, 要**检查栈是否为空, 如果非空返回false, 空的返回true.**

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
        for(char a : s)
        {
            if(isLeftBracket(a))
            {
                stk.push(a);
            }
            else
            {
                if(stk.empty())
                   return false; 
                bool doesMatch = isMatched(stk.top(), a);
                if(doesMatch)
                    stk.pop();
                else
                    return false;
            }
        }
        if(!stk.empty())
            return false;
        return true;
    }
private:
    bool isLeftBracket(char a)
    {
        if(a == '(' || a == '[' || a == '{')
            return true;
        return false;
    }
    bool isMatched(char left, char right)
    {
        if((left == '(' && right == ')') ||
           (left == '[' && right == ']') ||
           (left == '{' && right == '}'))
        {
            return true;
        }
        return false;
    }
};
```

