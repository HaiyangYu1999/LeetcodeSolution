Say you have an array for which the *i*th element is the price of a given stock on day *i*.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

**Example 1:**

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

**Example 2:**

```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

## 动态规划

设立两个变量 profit 和 min

对于i 从0 遍历到price.size()-1, profit存储了从0到i的最大利润，min存储了从0到i的最小值。

当i更新到i+1时，重新计算i+1是否是更小的值，与前面的min比较。同时计算i+1的加入是否产生了更大的profit。 price[i+1]-min与profit比较。

+ 此题可以这样做的关键在于，对于第i+1个加入的元素，只需要前i个元素中的最小值即可求出maxprofit，能在O(1)时间完成

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() == 0)
        {
            return 0;
        }
        int min = INT_MAX;
        int profit = INT_MIN;
        for(int i = 0; i < prices.size(); ++i)
        {
            if(prices[i] < min)
            {
                min = prices[i];
            }
            int profit_at_index_i = prices[i] - min;
            if(profit_at_index_i > profit)
            {
                profit = profit_at_index_i;
            }
        }
        return profit;
    }
};
```

