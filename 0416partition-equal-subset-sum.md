Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

 

**Example 1:**

```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**

```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```

 

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

## 1. 暴力(超时)

直接等价于用背包法找到和为sum/2的子数组

本来想的是这长度只有200, 暴力背包法应该能过的. 结果还是超时了. 看样子dfs还是不行. 必须30以内才稳

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int i : nums)
            sum += i;
        if((sum & 1) == 1)
            return false;
        int target = sum / 2;
        return doesPartitionSumEqualTarget(nums, target);
    }
    private boolean doesPartitionSumEqualTarget(int[] nums, int target)
    {
        return dfs(0, 0, nums, target);
    }
    private boolean dfs(int currSum, int currIndex, int[] nums, int target)
    {
        if(currSum > target || currIndex == nums.length)
            return false;
        if(currSum == target)
            return true;
        boolean addCurr = dfs(currSum + nums[currIndex], currIndex + 1, nums, target);
        if(addCurr)
            return true;
        boolean notAddCurr = dfs(currSum, currIndex + 1, nums, target);
        return notAddCurr;
    }
}
```

## 2 01背包

事实上我这个题一开始的想法是对的. 只是不知道01背包这样的问题怎么用动态规划. (事实上之前的背包问题我都是dfs做的. **本人大菜鸡一个**)

我们可以这样动态规划. 以下摘自https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/fen-ge-deng-he-zi-ji-by-leetcode-solution/.

`dp[i][j]` 表示从数组的 `[0,i]` 下标范围内选取若干个正整数（可以是 0 个），是否存在一种选取方案使得被选取的正整数的和等于 j。初始时，dp 中的全部元素都是 false。

首先确定边界条件. 由于任何范围内选取0个数, 都可以使得和为0. 故`dp[i][0] = true`对于任意的i均成立

对于`i = 0`, 由于只有2种情况. 选nums[0]和不选nums[0]所以`dp[0][0] = true`, `dp[0][nums[0]] = true`. 其他的`dp[0][j]`都为false.

下面推导状态转移方程.

对于`dp[i][j]`, 如果想让`dp[i][j]`为true, 有两种情况

+ 如果选nums[i], 那么就需要`dp[i-1][j - nums[i]]`为true. 同时要保证`j >= nums[i]`.

+ 如果不选nums[i], 就需要`dp[i-1][j]`为true.

上面两种情况只要有一个满足, 那么`dp[i][j]`就为true了.

同时, 我们发现dp[i]仅仅和dp[i-1]有关. 所以可以进一步优化空间复杂度到O(target).

下面给出优化过空间之后的代码

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int i : nums)
            sum += i;
        if((sum & 1) == 1)
            return false;
        int target = sum / 2;
        
        boolean[] dp_i = new boolean[target + 1];
        dp_i[0] = true;
        if(nums[0] > target) 
            return false;
        dp_i[nums[0]] = true;
        for(int i = 1; i < nums.length; ++i)
        {
            boolean[] dp_iPlus1 = new boolean[target + 1];
            for(int j = 0; j <= target; ++j)
            {
                if(nums[i] > j)
                {
                    dp_iPlus1[j] = dp_i[j];
                }
                else
                {
                    dp_iPlus1[j] = dp_i[j] || dp_i[j - nums[i]];
                }
            }
            dp_i = dp_iPlus1;
        }
        return dp_i[target];
    }
}
```

