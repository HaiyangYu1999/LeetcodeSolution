Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

**Example 1:**

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
```

**Example 5:**

```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

## 动态规划

这个题我自己没想出来, 思路完全借鉴于https://leetcode-cn.com/problems/regular-expression-matching/solution/zheng-ze-biao-da-shi-pi-pei-by-leetcode-solution/

`dp[i][j]` 表示的是`s[0]`到`s[i-1]`是否与`p[0]`到`p[j-1]`是否匹配.

状态转移矩阵的推到就不写了, 都在上面的URL中.

要注意的是, 当`p[j-1] == '*'`时, 并且`(p.charAt(j-2) == s.charAt(i-1) || p.charAt(j-2) == '.')`, 这时有两个选择, 去判断`dp[i-1][j]`和去判断`dp[i][j-2]`. 下面举例说明

> `s = "aaaa", p = "c*b*a*"` . 当检查到`p[5] == '*'`并且`p[3]== s[6] == 'a'`的时候, 我们可以选择p中最后的`a*`匹配0个a, 即检查新的`s = "aaaa", p = "c*b*"`是否匹配. 也可以选择p中最后的`a*`匹配s中最后一个a, 即检查新的`s = "aaa", p = "c*b*a*"`是否匹配. 这两个只要有一个匹配, 那么原来的s和p也是匹配的

同时, 要注意边界条件的判断,

+ `dp[0][0] = true`这时显然的
+ `dp[i][0] always false` 也是显然的`(i > 0)`
+ `dp[0][j]`比较难判断(**关键点是只有"`c*b*`"这样的正则表达式才能匹配0个字符的字串. 不以'*'结尾的正则表达式肯定不能匹配空串!**)
  + 当`p[j-1] != '*'`时, p要匹配含有至少1个字符的字符串. 显然是false
  + 当`p[j-1] == '*'`时, 使用这个星号匹配0个字符, 那么就有`dp[0][j] = dp[0][j-2]`. 例如, p = "ca*", s是空串. 首先检查p[2] 为星号, 使用这个星号匹配0个a, 那么即可检查新的p = "c"和空串是否匹配即可.

```java
class Solution {
    public boolean isMatch(String s, String p) {
        final int m = s.length();
        final int n = p.length();
        boolean[][] dp = new boolean[m+1][n+1];
        dp[0][0] = true;
        // dp[i][0] always false
        // but dp[0][j] depends, e.g. "a*" matches ""
        for(int i = 0; i <= m; ++i)
        {
            for(int j = 1; j <= n; ++j)
            {
                if(p.charAt(j-1) == '*')
                {
                    if(i != 0 && (p.charAt(j-2) == s.charAt(i-1) || p.charAt(j-2) == '.'))
                        dp[i][j] = dp[i-1][j] || dp[i][j-2];
                    else
                        dp[i][j] = dp[i][j-2];
                }
                else
                {
                    if(i == 0)
                    {
                        dp[i][j] = false;
                    }
                    else
                    {
                        if(p.charAt(j-1) == s.charAt(i-1) || p.charAt(j-1) == '.')
                            dp[i][j] = dp[i-1][j-1];
                        else
                            dp[i][j] = false;
                    }
                }
            }
        }
        return dp[m][n];
    }
}
```

