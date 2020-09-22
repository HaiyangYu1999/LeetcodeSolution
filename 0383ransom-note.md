Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

 

**Example 1:**

```
Input: ransomNote = "a", magazine = "b"
Output: false
```

**Example 2:**

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

**Example 3:**

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

 

**Constraints:**

- You may assume that both strings contain only lowercase letters.

## hashmap

首先要理解题意, 这题意我理解了半天. 被ransom note, magazine这些名词弄蒙了.

**这题的意思就是能不能从magazine的所有字符中挑选出一些字符来组成ransom note!**

先把magazine所有字符放一个(字符 -> 字符数量)map里面, 然后判断map里面的能不能组成ransom note即可.

但是使用哈希表只超过了5%的空间占用. 因为只有英文小写字母, 可以考虑用一个int[26]的数组来代替map, 空间占用应该会低一些

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int> mp;
        for(char c : magazine)
        {
            ++mp[c];
        }
        
        for(char c : ransomNote)
        {
            unordered_map<char, int>::iterator it = mp.find(c);
            if(it == mp.end())
                return false;
            else if(it->second <= 0)
                return false;
            else
            {
                --it->second;
            }
        }
        return true;
    }
};
```

