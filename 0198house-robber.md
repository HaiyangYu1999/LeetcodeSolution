You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

 

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

 

**Constraints:**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

## 1 动态规划

首先规定, dp[i] 代表 从i到n-1的子数组中, 可以抢劫(rob)的最大财产.

显然有dp[n-1] = nums[n-1], dp[n-2] = max(nums[n-2], nums[n-1])

下面根据dp[i+1]和dp[i+2]推导出dp[i]的表达式

对于从i到n-1的子数组中, 有2种情况, 

+ 第i家不抢, 那么第i+1及之后的这些家可抢可不抢. 所以从第i+1家到第n-1家抢的最大财产就是dp[i+1]
+ 抢第i家, 那么第i+1家一定不能抢(否则就进局子了), 而第i+2家及之后的这些可以自行决定抢不抢. 所以从第i+2家到第n-1家抢的最大财产就是dp[i+2], 加上抢的第i家的财产就是nums[i] + dp[i+2]

所以 `dp[i] = max(dp[i+1], nums[i] + dp[i+2])`

```java
class Solution {
    public int rob(int[] nums) {
        final int n = nums.length;
        if(n == 0) 
            return 0;
        if(n == 1)
            return nums[0];
        int[] dp = new int[n];
        dp[n-1] = nums[n-1];
        dp[n-2] = Math.max(nums[n-2], nums[n-1]);
        for(int i = n - 3; i >= 0 ; --i)
        {
            dp[i] = Math.max(dp[i+1], nums[i] + dp[i+2]);
        }
        return dp[0];
    }
}
```

## 2 动态规划(优化空间)

经过上面的分析, 计算dp[i]只需要dp[i+1]和dp[i+2], 发现根本不需要一整个数组来存储. 只需要3个变量储存当前i的值, i+1的值, i+2的值即可. 

多了一步的判断条件`if(n == 2)  return Math.max(nums[n-2], nums[n-1]);`是防止nums只有2个元素然后没有经过for循环, 返回了未计算的`dp_i`

```java
class Solution {
    public int rob(int[] nums) {
        final int n = nums.length;
        if(n == 0) 
            return 0;
        if(n == 1)
            return nums[0];
        if(n == 2)
            return Math.max(nums[n-2], nums[n-1]);
        int dp_iplus1 = Math.max(nums[n-2], nums[n-1]);
        int dp_iplus2 = nums[n-1];
        int dp_i = 0;
        for(int i = n - 3; i >= 0 ; --i)
        {
            dp_i = Math.max(dp_iplus1, nums[i] + dp_iplus2);
            dp_iplus2 = dp_iplus1;
            dp_iplus1 = dp_i;
        }
        return dp_i;
    }
}
```

