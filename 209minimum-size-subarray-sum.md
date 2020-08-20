Given an array of **n** positive integers and a positive integer **s**, find the minimal length of a **contiguous** subarray of which the sum ≥ **s**. If there isn't one, return 0 instead.

**Example:** 

```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

**Follow up:**

If you have figured out the *O*(*n*) solution, try coding another solution of which the time complexity is *O*(*n* log *n*). 

## 双指针

两个指针i,j记录子数组开始和结束.

+ 若子数组范围[i,j]的和sumTmp小于s, 那么j右移
+ 若子数组范围[i,j]的和sumTmp大于等于s, 记录此刻子数组的长度 j - i + 1, i右移

返回所有记录的长度中的最小值. 

```c++
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        if(nums.empty())
            return 0;
        int left = 0;
        int right = 0;
        int sumTmp = nums[0];
        int minLenGlobal = INT_MAX;
        while(right < nums.size())
        {
            if(sumTmp < s)
            {
                ++right;
                if(right == nums.size())
                    break;
                sumTmp += nums[right];
            }
            else
            {
                int tmpLen = right - left + 1;
                if(tmpLen < minLenGlobal)
                    minLenGlobal = tmpLen;
                sumTmp -= nums[left++];
            }
        }
        return minLenGlobal == INT_MAX ? 0 : minLenGlobal;
    }
};
```

