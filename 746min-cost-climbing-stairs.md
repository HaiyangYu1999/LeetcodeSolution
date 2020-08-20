On a staircase, the `i`-th step has some non-negative cost `cost[i]` assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

**Example 1:**

```
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
```



**Example 2:**

```
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
```



**Note:**

1. `cost` will have a length in the range `[2, 1000]`.
2. Every `cost[i]` will be an integer in the range `[0, 999]`.

## 动态规划

最经典的动态规划问题之一了. 

边界条件: 当i = 0 或 i= 1 时, 爬到第i层的花费为0

状态方程: 想要爬到第i层只有两种途径, 从i-1层开始爬和从i-2层开始爬, 从i-1爬的花费为cost1 = minCostClimbingStairs(i-1) + cost[i-1],  从i-2爬的花费为cost2 = minCostClimbingStairs(i-2) + cost[i-2], 所以爬到第i层的最小花费为min(cost1, cost2).

递归的求第cost.size()层的最小花费即可. 注意不要重复计算否则超时. 构造一个数组储存已经算出来的最小花费即可. 数组中有就直接用, 没有再递归的计算.

```c++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> costSum(cost.size() + 1, -1);
        costSum[0] = 0;
        costSum[1] = 0;
        return minCostClimbingStairs(cost, costSum, cost.size());
    }
private:
    int minCostClimbingStairs(const vector<int>& cost, vector<int>& costSum, int k)
    {
        if(costSum[k] != -1)
            return costSum[k];
        int fromMinus2 = minCostClimbingStairs(cost, costSum, k-2) + cost[k - 2];
        int fromMinus1 = minCostClimbingStairs(cost, costSum, k-1) + cost[k - 1];
        costSum[k] = min(fromMinus2, fromMinus1);
        return costSum[k];
    }
};
```

