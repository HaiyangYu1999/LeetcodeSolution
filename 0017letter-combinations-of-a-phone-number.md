Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note:**

Although the above answer is in lexicographical order, your answer could be in any order you want.

## DFS

直接使用DFS枚举出所有的可能性, 不需要任何条件的判断和剪枝.

```c++
class Solution {
public:
    Solution()
    {
        init();
    }
    vector<string> letterCombinations(string digits) {
        vector<string> ans;
        if(digits.empty()) return ans;
        string tmpString;
        letterCombinations(ans, tmpString, digits, 0);
        return ans;
    }
private:
    unordered_map<int, vector<char>> int2char;
    void init()
    {
        int2char.insert({2, vector<char>{'a','b','c'}});
        int2char.insert({3, vector<char>{'d','e','f'}});
        int2char.insert({4, vector<char>{'g','h','i'}});
        int2char.insert({5, vector<char>{'j','k','l'}});
        int2char.insert({6, vector<char>{'m','n','o'}});
        int2char.insert({7, vector<char>{'p','q','r','s'}});
        int2char.insert({8, vector<char>{'t','u','v'}});
        int2char.insert({9, vector<char>{'w','x','y','z'}});
    }
    void letterCombinations(vector<string>& ans,string& curr, const string& digits, int i)
    {
        if(i == digits.size())
        {
            ans.push_back(curr);
        }
        else
        {
            int tmp = digits[i] - '0';
            for(char ch : int2char[tmp])
            {
                curr += ch;
                letterCombinations(ans, curr, digits, i + 1);
                curr.pop_back();
            }
        }
    }
};
```

