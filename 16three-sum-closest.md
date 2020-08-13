Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

 

**Example 1:**

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

 

**Constraints:**

- `3 <= nums.length <= 10^3`
- `-10^3 <= nums[i] <= 10^3`
- `-10^4 <= target <= 10^4`

## 双指针扫描

和leetcode15思路基本相同, 甚至比leetcode15还简单(因为不用去重). 

先排序nums

设三个指针ijk

先固定i, j从i+1开始向右移动, k从nums.size() - 1开始向左移动. 当j == k时结束循环. 对于每一个i, j, k, 计算nums[i] + nums[j] + nums[k] 到target的距离, 并将历次的最小距离和对应的sum储存到minDist和minSum中. 

+ 当nums[i] + nums[j] + nums[k] == target时,直接返回0即可
+ 当nums[i] + nums[j] + nums[k] > target, --k. 因为如果k不递减的话, 只递增j, j越大sum越大, 到target的距离越大.不可能找到更优的解了.
+ 当nums[i] + nums[j] + nums[k] < target, ++j. 因为如果j不递增的话, 只递减k, k越小sum越小, 到target的距离越大.不可能找到更优的解了.

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int minDist = INT_MAX;
        int minSum = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            int a = nums[i];
            int j = i + 1;
            int k = nums.size() - 1;
            while(j < k)
            {
                int b = nums[j];
                int c = nums[k];
                int sum = a + b + c;
                int dist = abs(sum - target);
                if(minDist > dist)
                {
                    minDist = dist;
                    minSum = sum;
                }
                if(sum == target)
                    return sum;
                else if(sum < target)
                {
                    ++j;
                }
                else
                {
                    --k;
                }
            }
        }
        return minSum;
    }
};
```

