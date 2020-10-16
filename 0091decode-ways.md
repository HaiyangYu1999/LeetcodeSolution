A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number of ways to decode it.

The answer is guaranteed to fit in a **32-bit** integer.

 

**Example 1:**

```
Input: s = "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```
Input: s = "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

**Example 3:**

```
Input: s = "0"
Output: 0
Explanation: There is no character that is mapped to a number starting with '0'. We cannot ignore a zero when we face it while decoding. So, each '0' should be part of "10" --> 'J' or "20" --> 'T'.
```

**Example 4:**

```
Input: s = "1"
Output: 1
```

 

**Constraints:**

- `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zero(s).

## 动态规划

令`dp[i]`表示从0到i的字串有多少种解码方式.

`dp[0] = s.charAt(0) == '0' ? 0 : 1`

对于dp[i], 显然等于 `A*dp[i-1] + B*dp[i-2]`

其中, A代表第i个字符能否单独的成为一个解码单元, B代表第i-1个字符和第i个字符合起来能否成为一个解码单元.

还要考虑i-2是否小于0的问题.

还可以将空间复杂度化简, 因为dp[i]只和i-1, i-2有关.

```java
class Solution {
    public int numDecodings(String s) {
        if(s == null || "".equals(s) || "0".equals(s))
            return 0;
        if(s.length() == 1)
            return 1;
        int[] dp = new int[s.length()];
        dp[0] = s.charAt(0) == '0' ? 0 : 1;
        for(int i = 1; i < s.length(); ++i)
        {
            int dp_iMinus1 = s.charAt(i) == '0' ? 0 : dp[i-1];
            int dp_iMinus2 = 0;
            if(s.charAt(i-1) == '0' || s.charAt(i-1) - '0' > 2)
                dp_iMinus2 = 0;
            else if(s.charAt(i-1) - '0' == 2 && s.charAt(i) - '0' < 7)
                dp_iMinus2 = (i - 2 < 0) ? 1 : dp[i-2];
            else if(s.charAt(i-1) - '0' == 1)
                dp_iMinus2 = (i - 2 < 0) ? 1 : dp[i-2];
            dp[i] = dp_iMinus1 + dp_iMinus2;
        }
        return dp[s.length() - 1];
    }
}
```

