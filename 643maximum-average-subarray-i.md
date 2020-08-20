Given an array consisting of `n` integers, find the contiguous subarray of given length `k` that has the maximum average value. And you need to output the maximum average value.

**Example 1:**

```
Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
Explanation: Maximum average is (12-5-6+50)/4 = 51/4 = 12.75
```

 

**Note:**

1. 1 <= `k` <= `n` <= 30,000.
2. Elements of the given array will be in the range [-10,000, 10,000].

## 滑动窗口

直接从左到右依次出现的k个子数组的average即可

注意计算后一个窗口的sum可以利用前一个窗口的sum信息, sum = sum_previous - nums[i - 1] + nums[i - 1 + k]

 ```c++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        if(nums.size() == 0 || nums.size() < k)
        {
            throw string("InputException");
        }      
        int sumOfPrevious = accumulate(nums.begin(), nums.begin() + k, 0);
        double maxAvg = (double)sumOfPrevious / (double)k;
        for(int i = 1; nums.begin() + i + k - 1 != nums.end(); ++i)
        {
            int sumOfNow = sumOfPrevious - nums[i - 1] + nums[i + k - 1];
            sumOfPrevious = sumOfNow;
            double tmp = (double)sumOfNow / (double)k;
            if(tmp > maxAvg)
                maxAvg = tmp;
        }
        return maxAvg;
    }
};
 ```

