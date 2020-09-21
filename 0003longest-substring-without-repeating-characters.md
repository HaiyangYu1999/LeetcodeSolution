Given a string `s`, find the length of the **longest substring** without repeating characters.

 

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Example 4:**

```
Input: s = ""
Output: 0
```

 

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

## 滑动窗口 + hashmap

窗口边界为begin, end

维护一个hashmap, 储存着对应着从s[begin]到s[end]中所有字符及其在字符串中的位置

对于一个将要进入hashmap中的元素s[end+1], 检查hashmap中是否有这个字符, 如果没有, 将其插入到hashmap中, 并更新hashmap大小和更新end=end+1

如果hashmap中有s[end+1]这个值, 查找这个值出现的位置duplicateLocation, 然后移除掉hashmap中所有从s[begin]到s[duplicateLocation]的值, 最后更改begin = duplicateLocation + 1

时间复杂度 O(n)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int begin = 0;
        int end = 0;
        int maxLen = 0;
        unordered_map<char, int> mp;
        while(end != s.size())
        {
            unordered_map<char, int>::iterator it = mp.find(s[end]);
            if(it == mp.end()) // s[end] not in map
            {
                mp.insert({s[end],end});
                ++end;
                maxLen = max(end - begin, maxLen);
            }
            else
            {
                int duplicateLocation = it->second;
                for(int i = begin; i <= duplicateLocation; ++i)
                {
                    mp.erase(mp.find(s[i]));
                }
                begin = duplicateLocation + 1;
            }
        }
        return maxLen;
    }
};
```

