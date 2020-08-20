Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

## 1 hashmap

仿照two sum的做法, 构造一个map储存各个元素和各个元素出现的次数.

先对nums排序.

对于任意的i, j, i > j,这时有nums[i] <= nums[j]. 只需要在hashmap中寻找有没有值为-nums[i] - nums[j]的元素即可. 

关键在于如何去重. 下面记a=nums[i], b=nums[j] 因为nums是排好序的, 所以有a <= b

> 若map中有值为-nums[i] - nums[j]的元素c时, 若c  < b, 即c对应的数组下标一定在b对应的下标前面. 所以当a不变时, j在之前的遍历中b一定取值过现在的c, 所以现在一定会重复. 即(a, b, c) = (a, b_1, c_1) 其中b_1 = c, c_1 = b  例如[-4 -1 -1 0 0 1 1 2]中, (-1, 1, 0)和(-1, 0, 1)是重的. 所以一定要保证a<=b<=c的顺序.

> 若c > b 则没有重复的风险. 

>  若 c == b, 则要考虑c在map中出现的次数. 例如[-4 -1 -1 0 0 1 1 2]中, a = -4, b = 2, 想要找到c = 2,但是2只在nums出现一次. 所以不成立

> 还要考虑 a == b == c == 0的情况. 如果0出现的次数小于3次, 不成立.

> 同时也要避免递增i时出现的重复. [-4 **-1** -1 **0** 0 **1** 1 2] 和 [-4 -1 **-1** **0** 0 **1** 1 2] 是重的, 所以当`i > 0 && nums[i] == nums[i - 1]`时要continue

> 递增j时也可能出现重复. [-4 **-1** -1 **0** 0 **1** 1 2] 和 [-4 **-1** -1 0 **0** **1** 1 2]是重的. 所以当`j > i + 1 && nums[j] == nums[j - 1]`时要continue

这个方法时间为n^2, 空间为n

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> vvc;
        if(nums.size() < 3)
            return vvc;

        sort(nums.begin(), nums.end());

        unordered_map<int, int> mp;
        for(int i : nums)
        {
            ++mp[i];
        }

        for(int i = 0; i < nums.size(); ++i)
        {
            if(i > 0 && nums[i] == nums[i - 1])
            {
                continue;
            }
            int a = nums[i];
            for(int j = i + 1; j < nums.size(); ++j)
            {
                if(j > i + 1 && nums[j] == nums[j - 1])
                {
                    continue;
                }
                int b = nums[j];
                unordered_map<int, int>::iterator cIt = mp.find(-a - b);
                if(cIt == mp.end())
                {
                    continue;
                }
                int c = cIt->first;
                int c_count = cIt->second;
                if(c == b && c == a && c_count < 3)
                    continue;
                if(c == b && c_count < 2)
                {
                    continue;
                }
                if(c >= b)
                {
                    vector<int> tmp = {a,b,c};
                    vvc.push_back(tmp);
                }
            }
        }
        return vvc;
    }
};
```

## 2 扫描 + 双指针

固定 3 个指针中最左（最小）数字的指针 k，双指针 i，j 分设在数组索引 (k, len(nums))(k,len(nums)) 两端，通过双指针交替向中间移动，记录对于每个固定指针 k 的所有满足 nums[k] + nums[i] + nums[j] == 0 的 i,j 组合：
当 nums[k] > 0 时直接break跳出：因为 nums[j] >= nums[i] >= nums[k] > 0，即 3 个数字都大于 0 ，在此固定指针 k 之后不可能再找到结果了。
当 k > 0且nums[k] == nums[k - 1]时即跳过此元素nums[k]：因为已经将 nums[k - 1] 的所有组合加入到结果中，本次双指针搜索只会得到重复组合。
i，j 分设在数组索引 (k, len(nums))(k,len(nums)) 两端，当i < j时循环计算s = nums[k] + nums[i] + nums[j]，并按照以下规则执行双指针移动：
当s < 0时，i += 1并跳过所有重复的nums[i]；
当s > 0时，j -= 1并跳过所有重复的nums[j]；
当s == 0时，记录组合[k, i, j]至res，执行i += 1和j -= 1并跳过所有重复的nums[i]和nums[j]，防止记录到重复组合。

思路解析参考于[https://leetcode-cn.com/problems/3sum/solution/3sumpai-xu-shuang-zhi-zhen-yi-dong-by-jyd/](https://leetcode-cn.com/problems/3sum/solution/3sumpai-xu-shuang-zhi-zhen-yi-dong-by-jyd/)



```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> vvc;
        if(nums.size() < 3)
            return vvc;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size(); ++i)
        {
            if(i > 0 && nums[i] == nums[i-1])
            {
                continue;
            }
            if(nums[i] > 0)
                break;
            int a = nums[i];
            int j = i + 1;
            int k = nums.size() - 1;
            while(j < k)
            {
                if(j > i + 1 && nums[j] == nums[j-1])
                {
                    ++j;
                    continue;
                }
                if(k < nums.size() - 1 && nums[k] == nums[k+1])
                {
                    --k;
                    continue;
                }
                int b = nums[j];
                int c = nums[k];
                int sum = a + b + c;
                if(sum == 0)
                {
                    vector<int> tmp = {a,b,c};
                    vvc.push_back(tmp);
                    ++j;
                    --k;
                }
                else if(sum > 0)
                {
                    --k;
                }
                else
                {
                    ++j;
                } 
            }
        }
        return vvc;
    }
};
```

