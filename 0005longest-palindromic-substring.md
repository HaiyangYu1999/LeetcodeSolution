Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"
Output: "bb"
```

## 1 动态规划1(超时)

注意到, 从i到j的字串中的最长回文子串要么是他自己, 要么是从i+1到j字符串的回文子串, 要么是i到j-1字符串的回文子串.

即 

```c++
if string(i,j) is palindrome
    dp[i][j] = string(i,j);
else
    dp[i][j] = maxlen(dp[i+1][j], dp[i][j-1]);
```

但是这样做, 复杂度为n^3. (填充dp矩阵n^2, 每一次判断`string(i,j)`是不是回文需要n)
```c++
class Solution {
public:
    string longestPalindrome(string s) {
        if(s.empty()) return s;
        const int n = s.size();
        string dp[n][n];
        for(int i = 0; i < n; ++i)
        {
            dp[i][i] = string(s.begin() + i, s.begin() + i + 1);
        }
        
        for(int c = 1; c < n; ++c)
        {
            for(int i = 0; i < n - c; ++i)
            {
                if(isPalindrome(s, i, i + c))
                    dp[i][i+c] = string(s.begin() + i, s.begin() + i + c + 1);
                else
                {
                    string s1 = dp[i+1][i+c];
                    string s2 = dp[i][i+c-1];
                    dp[i][i+c] = (s1.size() > s2.size()) ? s1 : s2;
                }
            }
        }
        return dp[0][n-1];
    }
private:
    bool isPalindrome(const string& s, int begin, int end)
    {
        while(end > begin)
        {
            if(s[end] == s[begin])
            {
                ++begin;
                --end;
            }
            else
                return false;
        }
        return true;
    }
};
```



## 2 动态规划2

改变动态规划1中的做法, 我们存储一个bool数组`bool isPalindrome[i][j]`表示从i到j的字串是否为palindrome.

那么, 首先可以得到`isPalindrome[i][i] is always true`. `isPalindrome[i][i+1] = (s[i] == s[i+1])`

其次, `isPalindrome[i][j]`可以由 `isPalindrome[i+1][j-1]`推导出来. 

**如果`isPalindrome[i+1][j-1]`为真, 并且`s[i] == s[j]`, 那么就有`isPalindrome[i][j] = true`, 否则为false**

根据此结论即可递推的求出所有的`isPalindrome[i][j]`. 找最长的即可. 

时间和空间均为O(n^2)

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        if(s.empty()) return s;
        const int n = s.size();
        bool isPalindrome[n][n];
        for(int i = 0; i < n; ++i)
        {
            isPalindrome[i][i] = true;
        }
        for(int i = 0; i < n - 1; ++i)
        {
            isPalindrome[i][i+1] = (s[i] == s[i+1]);
        }
        
        for(int c = 2; c < n; ++c)
        {
            for(int i = 0; i < n - c; ++i)
            {
                isPalindrome[i][i+c] = (isPalindrome[i+1][i+c-1]) && (s[i] == s[i+c]);
            }
        }
        int maxLen = INT_MIN;
        int begin = 0;
        int end = 0;
        for(int i = 0; i < n; ++i)
        {
            for(int j = i; j < n; ++j)
            {
                if(isPalindrome[i][j])
                {
                    if(maxLen < j - i + 1)
                    {
                        maxLen = j - i + 1;
                        begin = i;
                        end = j;
                    }
                }
            }
        }
        return string(s.begin() + begin, s.begin() + end + 1);
    }
};
```

## 3 中心扩展法

考虑到每个回文子串都有一个中心, 这个中心可以是某个字符串中的字符, 也可能是某2个相邻字符中间处. 

所以一共有2n-1个可能的中心. 对于这2n-1个中心, 每一个中心都向外扩展, 看看最大能扩展到什么长度, 然后找出最大的就可以

时间复杂度O(n^2), 空间O(1)

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        if(s.empty()) return s;
        const int n = s.size();
        int maxLen = INT_MIN;
        int begin = 0;
        int end = 0;
        for(int mid = 0; mid < n; ++mid)
        {
            int localLen = 1;
            int localBegin = mid;
            int localEnd = mid;
            while(localBegin > -1 && localEnd < n && s[localBegin] == s[localEnd])
            {
                localLen = localEnd - localBegin + 1;
                if(maxLen < localLen)
                {
                    maxLen = localLen;
                    begin = localBegin;
                    end = localEnd;
                }
                --localBegin;
                ++localEnd;
            }
        }
        for(int mid = 0; mid < n - 1; ++mid)
        {
            int localLen = 0;
            int localBegin = mid;
            int localEnd = mid + 1;
            while(localBegin > -1 &&localEnd < n && s[localBegin] == s[localEnd])
            {
                localLen = localEnd - localBegin + 1;
                if(maxLen < localLen)
                {
                    maxLen = localLen;
                    begin = localBegin;
                    end = localEnd;
                }
                --localBegin;
                ++localEnd;
            }
        }
        return string(s.begin() + begin, s.begin() + end + 1);
    }
};
```

