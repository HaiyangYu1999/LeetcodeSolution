Given a sorted integer array without duplicates, return the summary of its ranges.

**Example 1:**

```
Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.
```

**Example 2:**

```
Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
```

## 双指针

指针begin记录continuous range第一个数, end记录continuous range的最后一个数. 若发现某个i和前面的不连续, 将begin和end统计成字符串存储到vector中, 并更新begin和end为nums[i]

```c++
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> vc;
        if(nums.empty())
            return vc;
        int begin = nums.front();
        int end = nums.front();
        for(int i = 1; ; ++i)
        {
            if(i < nums.size() && nums[i] == nums[i-1] + 1)
            {
                ++end;
            }
            else
            {
                if(begin == end)
                {
                    vc.push_back(to_string(begin));
                }
                else
                {
                    vc.push_back(to_string(begin) + "->" + to_string(end));
                }
                if(i == nums.size())
                    break;
                end = begin = nums[i];
            }
        }
        return vc;
    }
};
```

