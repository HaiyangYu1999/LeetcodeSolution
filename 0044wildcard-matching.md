Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `?` or `*`.

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
p = "*"
Output: true
Explanation: '*' matches any sequence.
```

**Example 3:**

```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```

**Example 4:**

```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```

**Example 5:**

```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

## 1 动态规划1

和leetcode 10 正则表达式匹配一样, 采用动态规划

思路可参考下面的URL

https://leetcode-cn.com/problems/wildcard-matching/solution/tong-pei-fu-pi-pei-by-leetcode-solution/

```java
class Solution {
    public boolean isMatch(String s, String p) {
        final int m = s.length() + 1;
        final int n = p.length() + 1;
        boolean[][] dp = new boolean[m][n];
        dp[0][0] = true; //empty patten always matches empty string
        for(int i = 1; i < m; ++i)
            dp[i][0] = false; // empty patten doesnt match non-empty string
        for(int j = 1; j < n; ++j)
        {
            if(p.charAt(j-1) == '*')
                dp[0][j] = dp[0][j-1];
            else
                dp[0][j] = false;
        }
        for(int i = 1; i < m; ++i)
        {
            for(int j = 1; j < n; ++j)
            {
                if(p.charAt(j-1) == '*')
                {
                    dp[i][j] = dp[i-1][j] || dp[i][j-1];
                }
                else
                {
                    if(p.charAt(j-1) == s.charAt(i-1) || p.charAt(j-1) == '?')
                        dp[i][j] = dp[i-1][j-1];
                    else
                        dp[i][j] = false;
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```

## 2 动态规划2

发现动态规划1中, 数组`dp[i]`可以完全由`dp[i-1]`推导, 所以可以只new2个1维数组, `dp_iMinus1[]`对应`dp[i-1][]`和`dp_i[]`对应`dp[i][]`

```java
class Solution {
    public boolean isMatch(String s, String p) {
        final int m = s.length() + 1;
        final int n = p.length() + 1;
        
        boolean[] dp_iMinus1 = new boolean[n];
        boolean[] dp_i = new boolean[n];
        dp_iMinus1[0] = true;
        for(int j = 1; j < n; ++j)
        {
            if(p.charAt(j-1) == '*')
                dp_iMinus1[j] = dp_iMinus1[j-1];
            else
                dp_iMinus1[j] = false;
        }
        
        for(int i = 1; i < m; ++i)
        {
            dp_i[0] = false;
            for(int j = 1; j < n; ++j)
            {
                if(p.charAt(j-1) == '*')
                {
                    dp_i[j] = dp_iMinus1[j] || dp_i[j-1];
                }
                else
                {
                    if(p.charAt(j-1) == s.charAt(i-1) || p.charAt(j-1) == '?')
                        dp_i[j] = dp_iMinus1[j-1];
                    else
                        dp_i[j] = false;
                }
            }
            dp_iMinus1 = Arrays.copyOf(dp_i, dp_i.length);
        }
        return dp_iMinus1[n-1];
    }
}
```

