Given a collection of integers that might contain duplicates, ***nums\***, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```



## 动态规划

本例和leetcode78相似, 但是关键在于去重. 如果使用leetcode78的方法后, 再排序去重, 时间复杂度会高(leetcode78返回的是规模为2\^n的vector, 排序去重操作使得总操作变为n*2\^n).

关键是将相同的元素看作一个整体!下面说明如何操作

+ 首先将nums排序, 构造一个statDup的映射, 统计相同的元素出现了多少次. 例如数组[1,2,2,3,3] 构造的statDup就为{(1,1), (2,2), (3,2)}

+ 假设0到nums[n]的子集s为所有0到nums[n]出现过的元素在数组中组成的集合, 例如n = 1, 所有出现过的元素为{1,2},{1,2}对应的数组范围为[1,2,2]. 所以所有子集s为[], [1], [2], [1,2], [1,2,2]

+ 当n = 2时所有出现过的元素仍为{1,2},所以所有子集仍为[], [1], [2], [1,2], [1,2,2], 也就是说,当nums[n] == nums[n-1]时, 0到nums[n]的子集就是0到nums[n-1]的子集

+ 当n=3时,这时`nums[n]  !=  nums[n-1]`, 我们根据之前statDup的计算结果,发现数组中有2个3, 所以构造附加集tmp = [], [3], [3,3], 对于0到nums[2]子集中的每一个元素, [], [1], [2], [1,2], [1,2,2], 都加上tmp中的一种([]或 [3]或 [3,3]).最后将这些结果合并, 即为所求

+ > tmp = []   subset of n-1 append tmp is [], [1], [2], [1,2], [1,2,2]
  >
  > tmp = [3]   subset of n-1 append tmp is [3], [1,3], [2,3], [1,2,3], [1,2,2,3]
  >
  > tmp = [3,3]   subset of n-1 append tmp is [3,3], [1,3,3], [2,3,3], [1,2,3,3], [1,2,2,3,3]
  >
  > 这三个append tmp之后的集合并起来即为所求

+ 当 n=4 时, nums[4],  = nums[3], 所以所求集合还是上面那些.

+ n遍历到数组末尾,即可求出结果

```c++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        unordered_map<int, int> statDup;
        //construct statDup
        for(int i : nums)
        {
            ++statDup[i];
        }
        return subsetsWithDup(nums, statDup, nums.size() - 1);
    }
private:
    vector<vector<int>> subsetsWithDup(vector<int>& nums, unordered_map<int, int>& statDup, int n)
    {
        vector<vector<int>> vvc;
        if(nums.size() == 0) return vvc;
        if(n == 0)
        {
            //find the first element count
            int cnt_0 = statDup[nums[0]];
            for(int i = 0; i <= cnt_0; ++i)
            {
                vector<int> tmp(i,nums[0]);
                vvc.push_back(tmp);
            }
            return vvc;
        }
        if(nums[n] == nums[n-1])
            return subsetsWithDup(nums, statDup, n-1);

        int cnt_n = statDup[nums[n]];
        vector<vector<int>> subsetBefore_n = subsetsWithDup(nums, statDup, n-1);

        //subset append nums[n]
        for(int i = 0; i <= cnt_n; ++i)
        {
            vector<int> tmp(i,nums[n]);
            //cannot be wirtten an for(auto& vc : subsetBefore_n) !!!!! think why?
            for(auto vc : subsetBefore_n)
            {
                vc.insert(vc.end(),tmp.begin(), tmp.end());
                vvc.push_back(vc);
            }
        }
        return vvc;
    }
};
```

