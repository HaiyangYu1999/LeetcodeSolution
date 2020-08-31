In a string `S` of lowercase letters, these letters form consecutive groups of the same character.

For example, a string like `S = "abbxxxxzyy"` has the groups `"a"`, `"bb"`, `"xxxx"`, `"z"` and `"yy"`.

Call a group *large* if it has 3 or more characters. We would like the starting and ending positions of every large group.

The final answer should be in lexicographic order.

 

**Example 1:**

```
Input: "abbxxxxzzy"
Output: [[3,6]]
Explanation: "xxxx" is the single large group with starting  3 and ending positions 6.
```

**Example 2:**

```
Input: "abc"
Output: []
Explanation: We have "a","b" and "c" but no large group.
```

**Example 3:**

```
Input: "abcdddeeeeaabbbcd"
Output: [[3,5],[6,9],[12,14]]
```

## 双指针

感觉不难, 直接双指针即可.  别忘了最后退出循环之后还要再判断一次最后的Group是不是LargeGroup.

```c++
class Solution {
public:
    vector<vector<int>> largeGroupPositions(string S) {
        vector<vector<int>> vvc;
        int begin = 0;
        int end = 0;
        while(end != S.size())
        {
            if(S[end] == S[begin])
                ++end;
            else
            {
                int groupLen = end - begin;
                if(groupLen >= 3)
                    vvc.push_back(vector<int>({begin,end - 1}));
                begin = end;
            }
        }
        assert(end == S.size());
        int groupLen = end - begin;
        if(groupLen >= 3)
            vvc.push_back(vector<int>({begin,end - 1}));
        return vvc;
    }
};
```

