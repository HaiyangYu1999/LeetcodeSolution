Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.

**Example 1:**

```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2:**

```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

## 1. split 0

首先要考虑nums只有一个元素, 返回nums.front()即可

若nums含有大于等于2个元素, 则最大积一定大于等于0.

将nums以0分割为几个子序列, 在这些子序列中选取最大的子数组乘积即可. 因为跨过2个子序列的子数组乘积一定为0

下面假设数组被0分割为几个子序列, [a1, a2, ..., an, **0**, b1,b2, ..., bm, **0**, c1,...].

index i遍历数组, 并用product记录子序列的乘积. `product = product * i ; nums[i] != 0;`

当nums[i] == 0时, 我们就获得了一段以0分隔的子序列. 记起点为begin, 终点为end, 乘积为product.

+ 若product > 0, 说明这一段含有0的子序列有偶数个负数. 此时product即为这一段子序列的最大子数组乘积.
+ 若product < 0, 说明有奇数个0, 我们需要从开头截掉一段含有第一个负数的子数组, 或者从结尾截掉一段含有最后一个负数的子数组. 例如[1,-1,-2,3,-2,4] 中, 含有第一个负数的子数组为[1, -1], 那么就令left = product/(-1\*1), 含有最后一个负数的子数组为[-2,4] right = product / (-2\*4). 返回left和right的最大值. 即比较[1,-1,-2,3]的乘积和[-2,3,-2,4]

最后, 把各段没有0的子序列的最大子数组乘积取最大值, 即为所求.

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        if(nums.size() == 1) return nums.front();
        int max0 = 0;
        vector<int>::iterator begin = nums.begin();
        vector<int>::iterator end = nums.begin();
        int product = 1;
        while(end != nums.end())
        {
            if(*end != 0)
            {
                product *= *end;
            }
            else
            {
                max0 = max(max0, maxProduct(begin, end, product));
                begin = end + 1;
                product = 1;
            }
            ++end;
        }
        max0 = max(max0, maxProduct(begin, nums.end(), product));
        return max0;
    }
private:
    int maxProduct(vector<int>::iterator begin, vector<int>::iterator end, int product)
    {
        if(begin == end)
            return 0; //no element
        if(end - begin == 1)
            return *begin; // one element
        if(product > 0)
            return product;
        else //product is negetive shows we must delete some negetive one in the product
        {
            int leftsubProduct = product;
            while(*--end > 0)
            {
                leftsubProduct /= *end;
            }
            leftsubProduct /= *end;
            int rightsubProduct = product;
            while(*begin > 0)
            {
                rightsubProduct /= *begin;
                ++begin;
            }
            rightsubProduct /= *begin;
            return max(leftsubProduct, rightsubProduct);
        }
    }
};
```

## 2 动态规划

用max[i] 表示以nums[i]结尾的最大乘积子数组的值. 用min[i] 表示以nums[i]结尾的最小乘积子数组的值.

则有 `max[i+1] = max(nums[i], nums[i] * max[i], nums[i] * min[i])`

`min[i+1] = min(nums[i], nums[i] * max[i], nums[i] * min[i])` **要考虑到一个负数和一串乘积为负数的子数组乘起来积很大， 和一个负数和一串乘积为正数的子数组乘积来的积很小**

利用一个变量maxGlobal 储存各个max[i]的最大值即可

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int min_prev = 1;
        int max_prev = 1;
        int maxGlobal = INT_MIN;
        for(int i = 0; i < nums.size(); ++i)
        {
            int min_now = min(nums[i], min(min_prev * nums[i], max_prev * nums[i]));
            int max_now = max(nums[i], max(min_prev * nums[i], max_prev * nums[i]));
            if(maxGlobal < max_now)
                maxGlobal = max_now;
            max_prev = max_now;
            min_prev = min_now;
        }
        return maxGlobal;
    }
};
```

