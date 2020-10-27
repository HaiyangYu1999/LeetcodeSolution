Given a string **s** and a **non-empty** string **p**, find all the start indices of **p**'s anagrams in **s**.

Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 20,100.

The order of output does not matter.

**Example 1:**

```
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```



**Example 2:**

```
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

## 改进的滑动窗口

普通的滑动窗口很简单, 就是从左到右依次取长度为pLen的字串, 比较和p是不是anagram.

但是这里可以改进一下, 

如果滑动窗口到一个新加入的char值, 这个值在p中并没有, 就可以跳过所有的包含这个值的窗口. 减少查找成本

> 例如 s: "cbaebabacd" p: "abc"
>
> 第一次查找时 **cba**ebabacd 为cba, 是anagram
>
> 下一次要判断bae是不是anagram.
>
> 我们发现新加进来的e在p中没有, 所以下面的三次查找直接跳过即可
>
>  c**bae**babacd, cb**aeb**abacd, cba**eba**bacd. 因为肯定是false.
>
> 直接从bab开始查找即可 cbae**bab**acd

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int pLen = p.length();
        if(s.length() < pLen)
            return ans;
        int[] pArr = getArray(p, 0, pLen - 1);
        int begin = 0;
        int end = pLen - 1;
        int[] curr = getArray(s, begin, end);
        while(end < s.length())
        {
            if(arrayEqual(curr, pArr))
            {
                ans.add(begin);
            }
            if(end == s.length() - 1)
                break;
            
            // chech if p contains s[end+1]
            // if p doesn't contain s[end+1]
            // skip all substring contains s[end+1]
            if(pArr[s.charAt(end + 1) - 'a'] == 0)
            {
                begin = end + 1;
                end = end + pLen;
                if(end >= s.length())
                    break;
                curr = getArray(s, begin, end);
                continue;
            }
            else
            {
                char beginChar = s.charAt(begin);
                --curr[beginChar - 'a'];
                ++begin;
                ++end;
                char endChar = s.charAt(end);
                ++curr[endChar - 'a'];
            } 
        }
        return ans;
    }
    private int[] getArray(String p, int begin, int end)
    {
        int[] ans = new int[26];
        for(int i = begin; i <= end; ++i)
        {
            ++ans[p.charAt(i) - 'a'];
        }
        return ans;
    }
    private boolean arrayEqual(int[] a, int[] b)
    {
        if(a.length != b.length)
            return false;
        for(int i = 0; i < a.length; ++i)
        {
            if(a[i] != b[i])
                return false;
        }
        return true;
    }
}
```

