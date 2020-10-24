Given a **non-empty** string *s* and a dictionary *wordDict* containing a list of **non-empty** words, determine if *s* can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

**Example 1:**

```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

## 1 DFS(超时)

s可以由wordDict组成的条件为, **对于`String i in wordDict`, 至少有一个字符串i使得s满足s由i开头, 并且s的剩余部分也能由wordDict组成**

递归边界为s为"".

不出所料, 超时了

超时用例

```
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaab"
["a","aa","aaa","aaaa","aaaaa","aaaaaa","aaaaaaa","aaaaaaaa","aaaaaaaaa","aaaaaaaaaa"]
```

**反思**: 上面的超时是因为进行了大量的无用计算.

> 例如, 计算"aaaab", dict = [a, aa]
>
> 首先选择dict其中的一个string "a", 接下来计算"aaab"是否可以由dict组成. 
>
> 然后计算"aaab"是否可以由dict组成的时候, 又要计算"aab"是否可以由dict组成. 以此类推最后得到**如果一开始选择"a", 不能由dict拼成这个字符串.**
>
> 但是, 我们回到起点, 接下来选择dict另外一个string "aa"作为开始, 接下来就又要判断"aab"是否可以由dict组成. 
>
> 这和前面两个"a"的情况一模一样, 所以就浪费了

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if(s == null || "".equals(s))
            return true;
        for(String i : wordDict)
        {
            if(s.startsWith(i) && wordBreak(s.substring(i.length()), wordDict))
                return true;
        }
        return false;
    }
}
```

## 2 动态规划

也是用dfs那个思路, 就是如果从0到i的子串可以由dict组成, 并且从i+1到j的子串在dict里面, 那么从0到j的子串也可以由dict组成.

边界是空串""是可以被dict组成.

dp[i] 表示从**0到i-1**的子串能否被dict组成. 

dp[0]表示空串. `dp[0] = true`

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if(s == null || "".equals(s))
            return true;
        final int n = s.length();
        boolean[] dp = new boolean[n+1];
        dp[0] = true;
        for(int i = 0; i < n; ++i)
        {
            if(dp[i] == false)
                continue;
            else
            {
                for(String word : wordDict)
                {
                    int j = word.length();
                    if(i + j <= n && s.substring(i, i + j).equals(word))
                    {
                        dp[i + j] = true;
                    }
                }
            }
        }
        return dp[n];
    }
}
```

