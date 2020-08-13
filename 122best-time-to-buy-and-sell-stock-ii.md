Say you have an array `prices` for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

**Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```

**Example 2:**

```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```

**Example 3:**

```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

 

**Constraints:**

- `1 <= prices.length <= 3 * 10 ^ 4`
- `0 <= prices[i] <= 10 ^ 4`

## 1 动态规划(超时)

思路是 维护一个数组maxSubProfit

maxSubProfit(index) 代表从0到index的子数组的最大profit

可以发现，子数组得到最大profit时必定是已经卖出的状态(因为假如到达index天还持有stocks，在index当天卖出后的profit一定大于这种情况)

所以，从第0天到第index+1天的最大profit可以用 第0天到第index天，第0天到第index-1天， ...  ，第0天到第0天表示为如下几者的最大值

+ 从第0天到第index - 1天的最大值 （因为与index只差一天，无法完成购买和卖出stocks的操作）
+ 从第0天到第i 天的profit最大值(i = index - 2 : 0) 加上第i + 1天买股票第index卖出获得的profit(如果这个不赚钱就加0)
+ 第0天买入中间不做任何操作，第index天卖出。（比较这个是因为防止stock价格一直递增导致的漏解）

将index设置为prices.size() - 1即可获得最终结果

此方法每计算一个index需要前面从0到index - 1的值来判断index次，复杂度O(n\^2)

（我自己以为这么复杂的题复杂度降低到n\^2已经可以了，暴力法要O(n*2\^n)的复杂度(列举所有的方法2\^n复杂度, 对每一种方法计算profit的平均复杂度为n/2)，事实上用O(n) 的复杂度就可以解决）


```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() == 0)
            return 0;
        vector<int> maxSubProfit(prices.size(), -1);
        return getSubMaxProfit(prices, maxSubProfit, prices.size() - 1);
        
    }
private:
    int getSubMaxProfit(vector<int>& prices, vector<int>& maxSubProfit, int index)
    {
        if(maxSubProfit[index] > -1)
        {
            return maxSubProfit[index];
        }
        if(index == 0)
        {
            maxSubProfit[index] = 0;
            return maxSubProfit[index];
        }
        if(index == 1)
        {
            maxSubProfit[index] = max(0, prices[1] - prices[0]);
            return maxSubProfit[index];
        }
        
        assert(index >= 2);
        
        int maxProfit = 0;
        for(int i = index - 1; i > 0; --i)
        {
            int profit_iBuy_indexSell = max(0,prices[index] - prices[i]);
            int total_profit = getSubMaxProfit(prices, maxSubProfit,i - 1) + profit_iBuy_indexSell;
            if(total_profit > maxProfit)
                maxProfit = total_profit;
        }
        
        int profit_indexMinus1 = getSubMaxProfit(prices,maxSubProfit,index - 1);
        if(maxProfit < profit_indexMinus1)
            maxProfit = profit_indexMinus1;
        int buy1_sellindex = prices[index] - prices[0];
        if(maxProfit < buy1_sellindex)
            maxProfit = buy1_sellindex;
        maxSubProfit[index] = maxProfit;
        return maxProfit;
    }
};
```

## 2 贪心算法

观察到要想赚的最多，只需要在所有上升的区间左边买入，右边卖出。为了防止卖出卖早了，所以要直到价格下跌的前一天才卖。价格下跌的当天就再买入，看看后面价格上升吗，如果不上升就撤销上一次买入，并且这一次再购买，上升就再等到价格下跌的前一天再卖出。这种做法对于任意一个价格上升的区间都能赚到，加起来显然是profit最多的。

这种方法相当于找到数组中所有递增的区间，时间复杂度显然是O(n)

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() == 0 || prices.size() == 1)
        {
            return 0;
        }
        //buy stocks at the first day
        int profit = -prices[0];
        int previous = prices[0];
        for(int i = 1; i < prices.size(); ++i)
        {
            bool isIncreasing = (previous < prices[i]);
            if(isIncreasing)  //if increase, don't sell wait the maximun price
            {
                ; //do nothing but refresh the previous var
                previous = prices[i];
            }
            else   //if found decrease, sell the stocks at yesterday's price and buy today's stock
            {
                profit += previous;
                profit -= prices[i];
                previous = prices[i];
            }
            bool isLastDay = (i == prices.size() - 1);
            //if today is the last day, stocks must be sold out, since you don't have anymore chance to sell them.
            if(isLastDay) 
            {
                profit += prices[i];
            }
        }
        return profit;
    }
};
```

