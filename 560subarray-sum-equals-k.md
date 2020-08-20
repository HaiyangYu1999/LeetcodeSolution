Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**.

**Example 1:**

```
Input:nums = [1,1,1], k = 2
Output: 2
```

 

**Constraints:**

- The length of the array is in range [1, 20,000].
- The range of numbers in the array is [-1000, 1000] and the range of the integer **k** is [-1e7, 1e7].

## 1 暴力(超时)

想不出来更好的了, 只能暴力

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int ans = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            int sum = 0;
            for(int j = i; j < nums.size(); ++j)
            {
                sum += nums[j];
                if(sum == k)
                    ++ans;
            }
        }
        return ans;
    }
};
```

## 2 Perfix Sum

**假如我们构造了一个数组S, 其中, S[i] 为前i项的和 nums[0] + nums[1] + ... + nums[i], 那么问题就转换为求数组S中的两个元素使得S[i] - S[j] = k 成立, 其中i > j>=-1. 这和Leetcode 1 Two Sum是不是完全相同!** 

所以构造hashmap, 为前i项的值映射到这个值的总个数. 

因为从j+1到i的子序列和可以表示为S[i]-S[j] `S[i]-S[j] = k => S[j] = S[i] - k`, 所以对于任意的S[i], 只需要寻找有多少个和为(S[i] - k)的前缀S[j], 即可求出以nums[i]结尾的, 和为k的子数组个数. 

注意, 我们定义S[-1] = 0;是因为对于从0到i的子序列和没有办法通过S[i] - S[j]的方法表示出来, 只能人为规定mp[0] = 1; (前缀和为0的最开始有1个)

同时为了保证j<i, 我们使用一次遍历的方法, 先求出S[i], 再计算map里面满足条件`S[j] = S[i] - k`的个数加到ans上(这时map里面的S[j]都能保证j<i), 最后再把S[i]放进map中. 

复杂度O(n) 

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int ans = 0;
        int sum = 0;
        unordered_map<int, int> mp;
        mp[0] = 1;
        for(int i = 0; i < nums.size(); ++i)
        {
            sum += nums[i];
            int tmp = mp[sum - k];
            ans += tmp;
            ++mp[sum];
        }
        return ans;
    }
};
```

