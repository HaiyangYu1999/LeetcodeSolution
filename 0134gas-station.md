There are *N* gas stations along a circular route, where the amount of gas at station *i* is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from station *i* to its next station (*i*+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

**Note:**

- If there exists a solution, it is guaranteed to be unique.
- Both input arrays are non-empty and have the same length.
- Each element in the input arrays is a non-negative integer.

**Example 1:**

```
Input: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

Output: 3

Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

**Example 2:**

```
Input: 
gas  = [2,3,4]
cost = [3,4,3]

Output: -1

Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

## 1 贪心1

首先很明显的一点就是 如果sum(gas) < sum(cost), 是没办法跑完一圈的.

然后, 我们构造一个新数组gas1, 其中gas1[i] = gas[i] - cost[i]

这个数组gas1表示了每经过一个station的消耗. 如果gas1[i] > 0, 就说明经过这个station的gas多于cost.

> 例如 gas  = [1,2,3,4,5], cost = [3,4,5,1,2], 那么新的gas1 = [-2,-2,-2, 3, 3]. 

对于上面的gas1, 我们显然应该要选择从第1个3出发, 这样才能积攒到足够多的油, 来支撑走完全程.

所以, 要从**循环数组中最大子序列的开始位置出发**, 只有这样才能积累足够多的油, 支撑跑完一圈.

下面证明这个结论

> 假设数组的最大子数组是从0开始, k结束. 数组其余部分是从k+1开始, 从n结束.
>
> 对于一般情况, 容易证明通过平移可以得到上面的形式.
>
> 我们用反证法, 
>
> 假如从最大子数组积累的油不够支撑到到达节点j, 即`sum(0,j) < 0` (j > k). 这里简记`sum(i,j)`为数组从第i个元素到第j个元素的求和. 
>
> 由于整个数组的和`sum(0, n) >= 0`(这是由`sum(gas) >= sum(cost)`保证的.)
>
> 而又有`sum(0,j) < 0`, 所以就能推出`sum(j+1,n) > 0`. 
>
> 既然`sum(j+1,n) > 0`, 那么从0到k的子数组就不是最大子数组了. 因为加上j+1到n这一段后会更大. 推出矛盾
>
> 所以只能是对于任意的j, (`k < j < n`), `sum(0,j) > 0` 也就是有足够的油跑到j. 

至于如何求循环数组的最大子序列的开始位置, 可以将两个gas1首尾连接拼成一个大的数组, 然后再进行普通的数组求最大子序列开始位置的计算. 

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int sum_gas = 0;
        int sum_cost = 0;
        for(int i : gas)
            sum_gas += i;
        for(int i : cost)
            sum_cost += i;
        if(sum_cost > sum_gas)
            return -1;
        for(int i = 0; i < gas.length; ++i)
            gas[i] -= cost[i];
        
        int[] doubleGas = new int[gas.length * 2];
        int k = 0;
        for(int i : gas)
            doubleGas[k++] = i;
        for(int i : gas)
            doubleGas[k++] = i;
        int maxSubarray = 0;
        int maxSubarrayBeginLocation = doubleGas.length - 1;
        int curr = 0;
        for(int i = doubleGas.length - 1; i > -1; --i)
        {
            curr = curr + doubleGas[i];
            if(curr > 0)
            {
                if(curr > maxSubarray)
                {
                    maxSubarray = curr;
                    maxSubarrayBeginLocation = i;
                }
            }
            else
            {
                curr = 0;
            }
        }
        return maxSubarrayBeginLocation % gas.length;
    }
}
```

## 2 贪心2

https://leetcode-cn.com/problems/gas-station/solution/guan-fang-ti-jie-kan-qi-lai-hao-fu-za-lai-kan-yi-k/

这个方法更巧妙, 用1次遍历就解决了所有的问题. 找到从0开始的最小子数组的结束位置i, 然后剔除掉这个子数组, 使得这个子数组最后才会被遍历. 从i+1开始.

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int endIndex = 0;
        int min = Integer.MAX_VALUE;
        int sumFrom0Toi = 0;
        for(int i = 0; i < gas.length; ++i)
        {
            int a = gas[i] - cost[i];
            sumFrom0Toi += a;
            if(sumFrom0Toi < min)
            {
                min = sumFrom0Toi;
                endIndex = i;
            }
        }
        return (sumFrom0Toi >= 0) ? (endIndex + 1) % gas.length : -1;
    }
}
```



