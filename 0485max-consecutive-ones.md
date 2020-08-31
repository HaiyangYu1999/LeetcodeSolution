Given a binary array, find the maximum number of consecutive 1s in this array.

**Example 1:**

```
Input: [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s.
    The maximum number of consecutive 1s is 3.
```



**Note:**

- The input array will only contain `0` and `1`.
- The length of input array is a positive integer and will not exceed 10,000

## 遍历

记录两个值, maxlen和currentlen, currentlen记录截止到当前出现过的最大长度, maxlen记录currentlen最大者

思路和最大子数组Leetcode53的动态规划解法相似

```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int maxLen = 0;
        int currentLen = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(nums[i] == 1)
            {
                ++currentLen;
                if(currentLen > maxLen)
                    maxLen = currentLen;
            }
            else
            {
                currentLen = 0;
            }
        }
        return maxLen;
    }
};
```

