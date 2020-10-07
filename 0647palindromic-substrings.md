Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

**Example 1:**

```
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

 

**Example 2:**

```
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

 

**Note:**

1. The input string length won't exceed 1000.

## 1 动态规划

这题和最长回文字串类似

通过动态规划求出任意的i,j, 字串s(i,j)是不是palindrome的. 然后再统计所有的palindrome字串个数.

时间复杂度O(n^2) 空间复杂度也是平方级别

```java
class Solution {
    public int countSubstrings(String s) {
        final int n = s.length();
        boolean[][] dp = new boolean[n][n];
        for(int i = 0; i < n; ++i)
            dp[i][i] = true;
        for(int i = 0; i < n - 1; ++i)
            dp[i][i+1] = (s.charAt(i) == s.charAt(i+1));
        for(int c = 2; c < n; ++c)
        {
            for(int i = 0; i + c < n; ++i)
            {
                dp[i][i+c] = dp[i+1][i+c-1] && (s.charAt(i) == s.charAt(i+c));
            }
        }
        int cnt = 0;
        for(int i = 0; i < n; ++i)
        {
            for(int j = i; j < n; ++j)
                if(dp[i][j])
                    ++cnt;
        }
        return cnt;
    }
}
```

## 2 中心扩展

最长回文字串的中心扩展法类似

空间复杂度减少到了O(1)

```java
class Solution {
    public int countSubstrings(String s) {
        final int n = s.length();
        int cnt = 0;
        for(int i = 0; i < n; ++i)
        {
            int left = i;
            int right = i;
            while(left > -1 && right < n)
            {
                if(s.charAt(left) == s.charAt(right))
                {
                    ++cnt;
                    --left;
                    ++right;
                }
                else
                    break;
            }
        }
        for(int i = 0; i < n - 1; ++i)
        {
            int left = i;
            int right = i + 1;
            while(left > -1 && right < n)
            {
                if(s.charAt(left) == s.charAt(right))
                {
                    ++cnt;
                    --left;
                    ++right;
                }
                else
                    break;
            }
        }
        return cnt;
    }
}
```

