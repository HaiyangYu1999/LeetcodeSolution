Given an unsorted array of integers, find the length of longest `continuous` increasing subsequence (subarray).

**Example 1:**

```
Input: [1,3,5,4,7]
Output: 3
Explanation: The longest continuous increasing subsequence is [1,3,5], its length is 3. 
Even though [1,3,5,7] is also an increasing subsequence, it's not a continuous one where 5 and 7 are separated by 4. 
```



**Example 2:**

```
Input: [2,2,2,2,2]
Output: 1
Explanation: The longest continuous increasing subsequence is [2], its length is 1. 
```



**Note:** Length of the array will not exceed 10,000.

## 双整数储存信息

构造两个int, maxLength和currentLength, i从1遍历到nums.size() - 1, maxLength储存截止到现在最长的递增子序列长度, currentLength储存子数组nums[0]到nums[i]中包含nums[i]的最长的递增子序列长度. 若currentLength > maxLength, 则更新maxLength. O(n)

```c++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.size() < 2)
            return nums.size();
        int maxLength = 1;
        int currentLength = 1;
        for(int i = 1; i < nums.size(); ++i)
        {
            if(nums[i] > nums[i - 1])
            {
                ++currentLength;
                if(currentLength > maxLength)
                    maxLength = currentLength;
            }
            else
            {
                currentLength = 1;
            }
        }
        return maxLength;
    }
};
```

