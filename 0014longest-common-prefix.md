Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Note:**

All given inputs are in lowercase letters `a-z`.

## 1 暴力

对strs的每个元素, 按位比较(这里的按位比较不是指位运算的那个位, 而是先比较第0个字符, 再比较第1个字符,..), 找到最长的前缀

复杂度mn, m是prefix的长度, n是strs的size

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string ans = "";
        if(strs.empty()) return ans;
        for(int i = 0; i < strs.front().size(); ++i)
        {
            char chr = strs.front()[i];
            bool isPrefix = true;
            for(string str : strs)
            {
                if(i >= str.size() || str[i] != chr)
                {
                    isPrefix = false;
                    break;
                }
            }
            if(!isPrefix)
                break;
            else
                ans.push_back(chr);
        }
        return ans;
    }
};
```

## 2 利用set排序

这个答案是引用 [leetcode.com,Author:[harsh_310]](https://leetcode.com/problems/longest-common-prefix/discuss/821706/4-ms-C%2B%2B-Solution-using-SET) 的, 思路是把所有的str放入一个 tree-set中.

那么有字典序 set[0] < set[1] < set[2] < ... < set[size - 1].

所以, 如果有 set[0] 和 set[size - 1] 有相同的某个最长前缀prefix, 那么根据字典序的关系, 所有的set元素的最长前缀也是prefix. 从而求出结果. 

时间复杂度 nlogn, 空间复杂度n

这个方法和暴力算法没有优劣之分, 因为不能确定logn 和 m的大小, 同时空间复杂度也比暴力高

```c++
class Solution {
public:
string longestCommonPrefix(vector<string>& strs) {
    int n = strs.size();
    if(n == 0)
        return "";
    if(n == 1)
        return strs[0];
    set<string> s;
    for(auto i : strs)
        s.insert(i);
    if(s.size() == 1)
        return *s.begin();
    else{
        string s1 = *s.begin();
        string s2 = *(s.rbegin());
        string temp = "";
        int string_size = min(s1.size(),s2.size());
        for(int i = 0; i < string_size;i++)
        {
            if(s1[i] != s2[i])
            return s1.substr(0, i);
        }
        return s1.substr(0, string_size);
    }
    return "";
    }
};
```

