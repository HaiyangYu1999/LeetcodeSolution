Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

**Example 2:**

```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

## 1 暴力

对于给定的i, 维护两个变量, left为左括号的数量, right为右括号的数量.

如果对于某个j, 从i到j的右括号数量大于左括号数量, 那么(i,j), (i, j+1), (i, j+2)....这些字串肯定不可能valid.

如果对于某个j, 从i到j的右括号数量等于左括号数量, 那么(i,j)就是valid, 更新maxLen = Math.max(left + right, maxLen);即可.

但是这种方法慢, 复杂度O(n^2)

```java
class Solution {
    public int longestValidParentheses(String s) {
        int maxLen = 0;
        for(int i = 0; i < s.length(); ++i)
        {
            int left = 0;
            int right = 0;
            for(int j = i; j < s.length(); ++j)
            {
                if(s.charAt(j) == '(')
                    ++left;
                else if(s.charAt(j) == ')')
                    ++right;
                if(right > left)
                    break;
                if(left == right)
                {
                    maxLen = Math.max(left + right, maxLen);
                }
            }
        }
        return maxLen;
    }
}
```

## 2 改进的括号判断

我们知道, 当从左到右遍历的时候, 如果右括号数量大于左括号数量, 后面无论括号是什么, 都不可能是一个合法的括号.

所以, i从0遍历到length - 1, 每遍历到一个括号, 都统计当前左右括号的数量. 

如果left < right, 那么当前的字串肯定不可能成为valid了, 需要从s[i]后面重新统计. 所以要重新赋值`left=right=0`

如果left = right, 那么当前的字串是valid的, 更新maxLen = right * 2.

例如, `())()()`

当遍历到第2个字符时, left = right = 1, 所以更新maxLen = 2;

当遍历到第3个字符时, left = 1, right =2, right > left, 所以`left=right=0`, 之前的合法括号`()`由于此次中断不能加入到后面的计算中.

但是这样做有个问题. 考虑`()(()()`, 一直有left > right, 导致遍历结束了还没有出现left == right, 自然不能更新maxLen. 

解决的方法是, 从右往左再遍历一遍, 判断方式和刚才的相反, 时刻保持右括号的数量大于左括号的数量.

取两次遍历结果的最大值.

```java
class Solution {
    public int longestValidParentheses(String s) {
        int left = 0;
        int right = 0;
        int maxLen = 0;
        for(int i = 0; i < s.length(); ++i)
        {
            if(s.charAt(i) == '(')
            {
                ++left;
            }
            else
                ++right;
            if(right > left)
            {
                right = 0;
                left = 0;
            }
            else if(right == left)
            {
                maxLen = Math.max(maxLen, 2 * right);
            }
        }
        left = 0;
        right = 0;
        for(int i = s.length() - 1; i > -1; --i)
        {
            if(s.charAt(i) == '(')
                ++left;
            else
                ++right;
            if(left > right)
            {
                right = 0;
                left = 0;
            }
            else if(right == left)
            {
                maxLen = Math.max(maxLen, 2 * right);
            }
        }
        return maxLen;
    }
}
```

## 3 动态规划

参考于https://leetcode-cn.com/problems/longest-valid-parentheses/solution/zui-chang-you-xiao-gua-hao-by-leetcode-solution/

```java
class Solution {
    public int longestValidParentheses(String s) {
        int[] dp = new int[s.length()];
        for(int i = 0; i < s.length(); ++i)
        {
            if(i == 0 || s.charAt(i) == '(')
            {
                dp[i] = 0;
            }
            else
            {
                if(i > 0 && s.charAt(i-1) == '(')
                    dp[i] = (i > 1) ? dp[i-2] + 2 : 2;
                else
                {
                    int tmp = i - 1 - dp[i-1];
                    if(tmp >= 0 && s.charAt(tmp) == '(')
                    {
                        dp[i] = dp[i-1] + 2 + ((tmp-1 > 0) ? dp[tmp-1] : 0);
                    }
                    else
                    {
                        dp[i] = 0;
                    }
                }
            }
        }
        int max = 0;
        for(int i : dp)
            max = Math.max(max,i);
        return max;
    }
}
```

