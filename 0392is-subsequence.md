Given a string **s** and a string **t**, check if **s** is subsequence of **t**.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

**Follow up:**
If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?

**Credits:**
Special thanks to [@pbrother](https://leetcode.com/pbrother/) for adding this problem and creating all test cases.

 

**Example 1:**

```
Input: s = "abc", t = "ahbgdc"
Output: true
```

**Example 2:**

```
Input: s = "axc", t = "ahbgdc"
Output: false
```

 

**Constraints:**

- `0 <= s.length <= 100`
- `0 <= t.length <= 10^4`
- Both strings consists only of lowercase characters.

## 1.双指针

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int i = 0;
        int j = 0;
        while(i != s.length() && j != t.length())
        {
            if(s.charAt(i) == t.charAt(j))
            {
                ++i;
                ++j;
            }
            else
            {
                ++j;
            }
        }
        return i == s.length();
    }
}
```

## 2. 动态规划

如果有很多个s, 只需要构造关于t的一些信息, 然后根据这些信息来查询.

很明显的信息就是一个字符c从第i个位置及之后第一次出现的位置.

记为`dp[i][c]`

然后只需要O(s.length)的时间复杂度就可以完成判断.

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        if(s.length() > t.length())
            return false;
        int[][] dp = new int[t.length()][26];
        // init dp[][]
        for(int i = 0; i != 26; ++i)
        {
            char ch = (char) (i + 'a');
            int index = -1;
            for(int j = t.length() - 1; j != -1; --j)
            {
                if(t.charAt(j) == ch)
                {
                    index = j;
                }
                dp[j][i] = index;
            }
        }
        int cursor = 0;
        for(int i = 0; i != s.length(); ++i)
        {
            if(cursor >= dp.length)
                return false;
            cursor = dp[cursor][s.charAt(i) - 'a'];
            if(cursor == -1)
                return false;
            cursor++;
        }
        return true;
    }
}
```

