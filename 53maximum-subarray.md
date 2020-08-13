Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

**Follow up:**

If you have figured out the O(*n*) solution, try coding another solution using the divide and conquer approach, which is more subtle.

## 1 暴力解法1(超时)

对于任意两个指针i, j  计算从nums[i]到nums[j]的和，找到最大的。

遍历i，j所花复杂度为 n(n+1)/2，对于所有的(i, j)，计算子数组和的总时间为A=sum_{i=0, n-1} ( sum_{j=i, n-1} ( j-i+1 ) ) = O(n\^3).平均计算子数组时间为2A/n(n+1)=O(n) 总复杂度为O(n\^3) 

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = nums[0];
        for(int i = 0; i < nums.size(); ++i)
        {
            for(int j = i; j < nums.size(); ++j)
            {
                int tmp = sum(i,j,nums);
                if(tmp > res)
                    res = tmp;
            }
        }
        return res;
    }
private:
    int sum(int i, int j, vector<int>& nums)
    {
        int res = 0;
        while(i<=j)
        {
            res += nums[i++];
        }
        return res;
    }
};
```

## 2 暴力解法2

观察到1中在计算sub sum的时候做了大量重复的计算。发现计算sum(i,j+1)时可以直接利用sum(i,j)的结果，可将计算sum的复杂度降为常数。总复杂度为O(n\^2)

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int sum = INT_MIN;           //考虑到数组中的所有元素可能为负，不能用0初始化
        for(int i = 0; i< nums.size(); ++i)
        {
            int sum_i = 0;
            for(int j = i; j < nums.size(); ++j)
            {
                sum_i += nums[j];
                if(sum_i > sum)
                {
                    sum = sum_i;
                }
            }
        }
        return sum;
    }
};
```

## 3 分治

**注意数组全部为负值的情况，此时只需要选一个最大的值即为所求**

观察到如果将数组[low , high]分为左右两部分，则最大子数组必然出现在3种情况种

+ 全部在左边的数组中

+ 全部在右边的数组中

+ 一部分在左边数组一部分在右边数组，并且必然包含边界元素

此时记数组边界为[low , high]，分界符mid = low + (high - low) / 2， 则左数组边界为[low, mid]，右数组边界为[mid+1, high]（至于为什么不采用边界[low, mid - 1] 和 [mid, high]，当原始数组只有2个元素时会出现错误）

对于前两种情况，分别递归调用边界为[low, mid]和[mid+1, high]的函数，注意边界条件为数组只有1个元素时跳出递归。只要原始数组nums不是0个元素，此划分不会出现边界问题。

  第三种情况，分别计算左半部分包含边界mid的最大和（nums[i]+...+nums[mid], i from low to mid）和右半部分包含边界mid+1的最大和（nums[mid+1]+...+nums[j], j from mid+1 to high），最大和为两者相加

最后返回三种情况的最大值

对于第三种情况，计算最大子序列的复杂度为O(n)

所以 T(n) = 2T(n/2) + O(n)，总复杂度为O(nlogn)

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max = *max_element(nums.begin(), nums.end());
        if(max <= 0)   //if all elements less than 0, return max(nums)
        {
            return max;   
        }
        if(nums.size() == 0)
        {
            throw string("null array");
        }
        return dividedMaxSubArray(nums, 0, nums.size() - 1);
    }
private:
    int dividedMaxSubArray(vector<int>& nums, int low, int high)
    {
        if(high == low)   //only one value in subarray
        {
            return max(nums[low],0);
        }
        int mid = low + (high - low) / 2;
        
        int leftMax = dividedMaxSubArray(nums, low, mid);
        int rightMax = dividedMaxSubArray(nums, mid + 1, high);
        
        int leftHalf = 0;
        int leftTmp = 0;
        for(int i = mid; i >= low; --i)
        {
            leftTmp += nums[i];
            if(leftTmp > leftHalf)
                leftHalf = leftTmp;
        }
        
        int rightHalf = 0;
        int rightTmp = 0;
        for(int j = mid + 1; j <= high; ++j)
        {
            rightTmp += nums[j];
            if(rightTmp > rightHalf)
                rightHalf = rightTmp;
        }
        
        int midMax = leftHalf + rightHalf;
        return max(midMax,max(leftMax,rightMax));
    }
};
```

## 4 动态规划

同样要先处理数组全是负数的情况

**观察到，如果一个子序列和为负数，那么这个子序列一定不可能出现在最大子序列中。**

设立两个变量 max，tmp，对于i from 0 to nums.size()

tmp记录了从0到i中，所有以nums[i]结尾的子数组的最大值，因为如果存在在i前面的j，和j前面的任意k，使得从k到j的和为负，那么从k到i的和一定小于j到i的和。所以在遍历到j的时候就应该置tmp为0，不要j前面这一段。

对于从0到n的每个i 依次将max与tmp比较，取这些tmp的最大值，就得到了每个以nums[i]结尾的子序列的最大值中的最大值。

**dp[i+1] = max(dp[i]+nums[i+1], nums[i+1])** 其中dp[i]表示以nums[i]结尾的最大子数组

只遍历一遍，所以总复杂度为O(n)

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxElem = *max_element(nums.begin(), nums.end());
        if(maxElem < 0)
            return maxElem;
        int max = 0;
        int tmp = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            tmp += nums[i];
            if(tmp < 0)
            {
                tmp = 0;
            }
            if(tmp > max)
                max = tmp;
        }
        return max;
    }
};
```

