Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

 

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

**Example 4:**

```
Input: s = "  Bob    Loves  Alice   "
Output: "Alice Loves Bob"
```

**Example 5:**

```
Input: s = "Alice does not even like bob"
Output: "bob like even not does Alice"
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
- There is **at least one** word in `s`.

 

**Follow up:**

- Could you solve it **in-place** with `O(1)` extra space?

 ## 1 双指针

用双指针从后向前遍历每一个单词, 逐个加到一个空串中. 原地修改的哪种方法实在想不出来了. 还是菜🥗🥗🥗

```c++
class Solution {
public:
    string reverseWords(string s) {
        if(s == "")
            return s;
        int i = s.size() - 1;
        int j = s.size() - 1;
        string ans = "";
        while(i > -1)
        {
            if(s[i] == ' ' && s[j] == ' ')
            {
                --i;
                --j;
            }
            else
            {
                while( i > -1 && s[i] != ' ')
                    --i;
                ans.insert(ans.end(), s.begin() + i + 1, s.begin() + j + 1);
                ans.push_back(' ');
                j = i;
            }
        }
        return string(ans.begin(), ans.end() - 1);
    }
};
```

## 2 两次翻转

先把字符串所有字符都反转, 然后再对于每一个单词翻转. 最后去除多余的空格.

这么简单的思路竟然没想到, 太不应该了

```c++
class Solution {
public:
    string reverseWords(string s) {
        if(s == "")
            return s;
        wholeReverse(s);
        partialReverse(s);
        trimSpace(s);
        return s;
    }
private:
    void wholeReverse(string& s)
    {
        int i = 0;
        int j = s.size() - 1;
        while(i < j)
        {
            std::swap(s[i], s[j]);
            ++i;
            --j;
        }
    }
    void partialReverse(string& s)
    {
        int i = 0;
        int j = 0;
        while(i < s.size())
        {
            if(s[i] == ' ')
            {
                ++i;
                ++j;
            }
            else
            {
                while(j < s.size() && s[j] != ' ')
                {
                    ++j;
                }
                int begin = i;
                int end = j - 1;
                while(begin < end)
                {
                    std::swap(s[begin], s[end]);
                    ++begin;
                    --end;
                }
                i = j;
            }
        }
    }
    void trimSpace(string& s)
    {
        int i = 0;
        int j = 0;
        for(int j = 0; j < s.size(); ++j)
        {
            if(s[j] != ' ')
            {
                s[i++] = s[j];
            }
            else
            {
                if(j == 0)
                    continue;
                else if(s[j] == ' ' && s[j-1] == ' ')
                    continue;
                else
                    s[i++] = s[j];
            }
        }
        s.erase(s.begin() + i, s.end());
        // erase the last space if exists
        // because "a   " becomes "a " after above process
        if(s.back() == ' ')
            s.pop_back();
    }
};
```

