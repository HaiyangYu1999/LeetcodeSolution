You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

 

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 3:**

```
Input: nums = [0]
Output: 0
```

 

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

## 动态规划

和leetcode 198 一样, 只是多了个条件, 第一家和最后一家不能同时抢.

所以分别计算nums从0到n-1的子数组最大抢劫数量, 和从1到n的子数组最大抢劫数量, 两者取最大值即可

同leetcode 198, 这里的第i个状态只和第i-1个状态和第i-2个状态有关, 所以不需要一个数组来储存. 单独设两个变量即可

```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 0)
            return 0;
        if(nums.length == 1)
            return nums[0];
        if(nums.length == 2)
            return Math.max(nums[0], nums[1]);
        int[] dp0 = new int[nums.length - 1];
        int[] dp1 = new int[nums.length - 1];
        dp0[0] = nums[0];
        dp0[1] = Math.max(nums[0], nums[1]);
        for(int i = 2; i < nums.length - 1; ++i)
        {
            dp0[i] = Math.max(dp0[i-1], dp0[i-2] + nums[i]);
        }
        dp1[0] = nums[1];
        dp1[1] = Math.max(nums[1], nums[2]);
        for(int i = 2; i < nums.length - 1; ++i)
        {
            dp1[i] = Math.max(dp1[i-1], dp1[i-2] + nums[i+1]);
        }
        return Math.max(dp0[nums.length - 2], dp1[nums.length - 2]);
    }
}
```

