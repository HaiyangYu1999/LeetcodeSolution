Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```



## 动态规划

关键点是 **假如已经找到数组[nums[0],...,nums[n-1]]的全部子集(记为s), 那么数组[nums[0], ..., nums[n]]的全部子集就等于s中所有元素加上nums[n]产生的集合 ∪ 集合s**

例如, [1,2,3]中前2个元素的全部子集s为{[], [1], [2], [1, 2]}

那么前2个元素的全部子集s中每个元素加上3产生的集合为{[3], [1,3], [2,3], [1,2,3]}

并上s = {[], [1], [2], [1, 2]}

所以前3个元素的子集为[], [1], [2], [1, 2], [3], [1,3], [2,3], [1,2,3]

复杂度分析,由于每一次计算0到n都需要先计算0到n-1的结果res(res中有2\^(n-1)个元素), 然后再花费2^(n-1)将res的结果添加到0

到n的结果中,所以`T(n) = T(n - 1) + O(2^(n-1))`, 即2^n的复杂度

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        return subsets(nums, nums.size() - 1);
    }
private:
    vector<vector<int>> subsets(vector<int>& nums,int n) {
        vector<vector<int>> vvc;
        if(nums.size() == 0)
            return vvc;
        if(n == 0)
        {
            vvc.push_back(vector<int>());
            vvc.push_back(vector<int>{nums[0]});
            return vvc;
        }
        else
        {
            auto subsetWithoutNums_n = subsets(nums, n - 1);
            for(const auto & vectorWithout_n : subsetWithoutNums_n)
            {
                vvc.push_back(vectorWithout_n);
                vector<int> vectorWith_n(vectorWithout_n.cbegin(), vectorWithout_n.cend());
                vectorWith_n.push_back(nums[n]);
                vvc.push_back(vectorWith_n);
            }
        }
        return vvc;
    }
};
```

