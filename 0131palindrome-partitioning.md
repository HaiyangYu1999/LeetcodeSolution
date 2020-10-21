Given a string *s*, partition *s* such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of *s*.

**Example:**

```
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```

## 1 动态规划

**这道题感觉和leetcode 93差不多, 都是那种求所有可能的解的. 用了动态规划更慢, 还是老老实实用DFS吧**

**只有那种求最大, 求最小的问题用动态规划才合适.**

dp[i]表示从0到i的字串所形成的所有palindrome partitioning

例如 对于"aab" `dp[0] = [[a]], dp[1] = [[a,a], [aa]], dp[2] = [[a,a,b], [aa,b]]`

那么如果求出了dp[0]到dp[i-1], 就可以根据这些信息生成dp[i]. 步骤如下

> 如果从0到i的子串是回文串, 就把这个子串单独组成的list加入到dp[i]中
>
> 如果从j到i的字串是回文串(j > 0), 首先找到从0到j-1的所有回文列表, 每个列表都加上从j到i的字串`string.substring(j, i+1)` 然后将这些列表add到dp[i]中.

```java
class Solution {
    public List<List<String>> partition(String s) {
        if(s == null || "".equals(s))
            return new ArrayList<>();
        final int n = s.length();
        List<List<String>>[] dp = (List<List<String>>[]) new List[n];
        List<List<String>> dp0 = new ArrayList<>();
        dp0.add(Arrays.asList("" + s.charAt(0)));
        dp[0] = dp0;
        for(int i = 1; i < n; ++i)
        {
            List<List<String>> currLists = new ArrayList<>();
            if(isPalindromic(s.substring(0, i + 1)))
            {
                currLists.add(Arrays.asList(s.substring(0, i + 1)));
            }
            for(int j = 1; j <= i; ++j)
            {
                if(isPalindromic(s.substring(j, i + 1)))
                {
                    List<List<String>> prevLists = dp[j-1];
                    for(List<String> prevList : prevLists)
                    {
                        List<String> currList = new ArrayList<>(prevList);
                        currList.add(s.substring(j, i + 1));
                        currLists.add(currList);
                    }
                }
                
            }
            dp[i] = currLists;
        }
        return dp[n-1];
        
    }
    
    private boolean isPalindromic(String s)
    {
        if(s == null)
            return false;
        if(s.length() <= 1)
            return true;
        int i = 0;
        int j = s.length() - 1;
        while(i < j)
        {
            if(s.charAt(i) != s.charAt(j))
                return false;
            ++i;
            --j;
        }
        return true;
    }
}
```

## 2 DFS

直接用DFS寻找所有的可能. 看上去是暴力解法, 但是这是最快的了.

```java
class Solution {
    public List<List<String>> partition(String s) {
        if(s == null || "".equals(s))
            return new ArrayList<>();
        List<List<String>> ans = new ArrayList<>();
        List<String> curr = new ArrayList<>();
        partition(ans, curr, s);
        return ans;
    }
    
    private void partition(List<List<String>> ans, List<String> curr, String s)
    {
        if("".equals(s))
        {
            ans.add(new ArrayList<>(curr));
        }
        else
        {
            for(int i = 0; i < s.length(); ++i)
            {
                if(isPalindromic(s.substring(0, i + 1)))
                {
                    curr.add(s.substring(0, i + 1));
                    partition(ans, curr, s.substring(i+1, s.length()));
                    curr.remove(curr.size() - 1);
                }
            }
        }
    }
    
    private boolean isPalindromic(String s)
    {
        if(s == null)
            return false;
        if(s.length() <= 1)
            return true;
        int i = 0;
        int j = s.length() - 1;
        while(i < j)
        {
            if(s.charAt(i) != s.charAt(j))
                return false;
            ++i;
            --j;
        }
        return true;
    }
}
```

