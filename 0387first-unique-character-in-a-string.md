Given a string, find the first non-repeating character in it and return its index. If it doesn't exist, return -1.

**Examples:**

```
s = "leetcode"
return 0.

s = "loveleetcode"
return 2.
```

 

**Note:** You may assume the string contains only lowercase English letters.



## 哈希表

先存储s中的所有字符的个数, 然后i从0开始找到第一个使得s[i]的个数为1的i, 返回i即可

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char, int> mp;
        for(char i : s) ++mp[i];
        for(int i = 0; i < s.size(); ++i)
        {
            auto it = mp.find(s[i]);
            if(it->second == 1)
            {
                return i;
            }
        }
        return -1;
    }
};
```

