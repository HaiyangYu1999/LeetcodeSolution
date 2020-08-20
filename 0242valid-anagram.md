Given two strings *s* and *t* , write a function to determine if *t* is an anagram of *s*.

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

**Note:**
You may assume the string contains only lowercase alphabets.

**Follow up:**
What if the inputs contain unicode characters? How would you adapt your solution to such case?

## hashmap

构造2个hashmap 存放s中哪个字符出现多少次, t中哪个字符出现多少次

然后判断这两个hashmap是否相等即可

官方题解是构造整个字符集大小的数组储存出现次数, 个人感觉不是太好. 如果是unicode肯定gg

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size() != t.size())
            return false;
        unordered_map<char,int> smap;
        unordered_map<char,int> tmap;
        for(char i : s)
        {
            ++smap[i];
        }
        for(char i : t)
        {
            ++tmap[i];
        }
        for(const auto & i : smap)
        {
            if(i.second != tmap[i.first])
                return false;
        }
        return true;
    }
};
```

