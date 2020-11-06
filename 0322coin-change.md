You are given coins of different denominations and a total amount of money *amount*. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

 

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**

```
Input: coins = [1], amount = 0
Output: 0
```

**Example 4:**

```
Input: coins = [1], amount = 1
Output: 1
```

**Example 5:**

```
Input: coins = [1], amount = 2
Output: 2
```

 

**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

## 动态规划

回溯是肯定不行的, 太慢了.如果碰到这样的用例, coins = [1,100], amount = 10000就惨了.

首先创建一个dp数组`int[] dp = new int[amount + 1];`

`dp[i]`表示组成`amount` 为 `i`的所用最少硬币数. 初始时数组全部为-1.

首先,  边界条件是`dp[0] = 0`

对于`dp[i]`, 想要找到组成i所需要的最小硬币数, 假设最后一个组成i的硬币为j, 那么通过j组成i的硬币数就为`dp[i - j] + 1`. 前提是`i - j > 0`并且`dp[i - j] != -1`.

对于在coins中的所有满足上述条件的j, 选取一个使得`dp[i - j] + 1`最小的j, 即为`dp[i]`



```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        for(int i = 0; i < dp.length; ++i)
            dp[i] = -1;
        dp[0] = 0;
        for(int i = 1; i < dp.length; ++i)
        {
            int minCoins = Integer.MAX_VALUE;
            for(int j : coins)
            {
                int tmp = i - j;
                if(tmp < 0 || dp[tmp] == -1)
                    continue;
                else
                {
                    int currCoins = dp[tmp] + 1;
                    minCoins = Math.min(minCoins, currCoins);
                }
            }
            dp[i] = minCoins == Integer.MAX_VALUE ? -1 : minCoins;
        }
        return dp[amount];
    }
}
```

