You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

**Example 1:**

```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

 

**Constraints:**

- The length of the given array is positive and will not exceed 20.
- The sum of elements in the given array will not exceed 1000.
- Your output answer is guaranteed to be fitted in a 32-bit integer.

## 1 回溯

其实回溯这个名字吧, 说的好听叫回溯, 说的不好听就叫穷举出所有可能.

第一次做这个题只想到了回溯, 没有想到动态规划的方法. 

```java
class Solution {
    private int ans = 0;
    public int findTargetSumWays(int[] nums, int S) {
        findTargetSumWays(0, 0, nums, S);
        return ans;
    }
    private void findTargetSumWays(int currSum, int currIndex, int[] nums, int S){
        if(currIndex == nums.length){
            if(S == currSum){
                ++ans;
                return;
            }
            else
                return;
        }
        findTargetSumWays(currSum + nums[currIndex], currIndex + 1, nums, S);
        findTargetSumWays(currSum - nums[currIndex], currIndex + 1, nums, S);
    }
}
```

## 2 动态规划

这个动态规划还真不容易想到. 要把答案放在数组里. `dp[i][j]`表示从nums[0]到nums[i]的子数组中可以组成和为j的方案数. 那么就有`dp[i][j] = dp[i - 1][j - nums[i]] + dp[i - 1][j + nums[i]]`. 

同时因为下标不能是负数, 要把第二维的所有下标都加上sum.

```java
class Solution {
    int[][] dp;
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for(int i : nums)
            sum += i;
        final int M = sum;
        dp = new int[nums.length][2 * M + 1];
        for(int i = 0; i < nums.length; ++i){
            if(i == 0){
                dp[i][nums[i] + M] += 1;
                dp[i][-nums[i] + M] += 1;
            }
            else{
                for(int j = -sum; j <=  sum; ++j){
                    int a1 = (j - nums[i] < -sum) ? 0 : dp[i-1][j - nums[i] + M];
                    int a2 = (j + nums[i] > sum) ? 0 : dp[i-1][j + nums[i] + M];
                    dp[i][j + M] += a1 + a2;
                }
            }
        }
        return S > M || S < -M ? 0 : dp[nums.length - 1][S + M];
    }
}
```

