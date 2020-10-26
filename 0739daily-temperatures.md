Given a list of daily temperatures `T`, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put `0` instead.

For example, given the list of temperatures `T = [73, 74, 75, 71, 69, 72, 76, 73]`, your output should be `[1, 1, 4, 2, 1, 1, 0, 0]`.

**Note:** The length of `temperatures` will be in the range `[1, 30000]`. Each temperature will be an integer in the range `[30, 100]`.

## 1 辅助数组

注意到, 温度是从30到100的. 为了方便起见, 我们就假设从0到100好了.

所以设置一个数组`int[101] last`. `last[t]`表示超过温度t的所有天数中, 最近的(最靠前的)一天的日期

数组T从后向前遍历, `for(int i = T.length - 1; i > -1; --i)`, 首先获得当日的温度`tmp = T[i]`

如果last[tmp] = 0, 就说明从i往后的超过温度tmp的日期不存在, 返回0.

如果`last[tmp] == x != 0`, 就说明在第x天的温度超过了第i天的温度tmp, 此时结果应该为`x - i`

并且要更新数组last, 把last[j] 全部更新为i, 其中j为小于tmp的所有值. 表明温度大于j的所有天数中, 最靠前的一天是第i天

时间复杂度为O(m*n). m是所有可能的温度跨度

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] ans = new int[T.length];
        int[] last = new int[101];
        for(int i = T.length - 1; i > -1; --i)
        {
            ans[i] = (last[T[i]] == 0) ? 0 : last[T[i]] - i;
            for(int j = 0; j < T[i]; ++j)
            {
                last[j] = i;
            }
        }
        return ans;
    }
}
```

## 2 单调栈

这个解法还不错. 只不过之前从来没接触过单调栈的肯定想不到

参考于 https://leetcode-cn.com/problems/daily-temperatures/solution/java-by-sdwwld/

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] ans = new int[T.length];
        Deque<Integer> stk = new ArrayDeque<>();
        for(int i = 0; i < T.length; ++i)
        {
            if(stk.isEmpty())
            {
                stk.offerFirst(i);
            }
            else
            {
                while(!stk.isEmpty() && T[i] > T[stk.peekFirst()])
                {
                    int j = stk.pollFirst();
                    ans[j] = i - j;
                }
                stk.offerFirst(i);
            }
        }
        return ans;
    }
}
```

## 3 伪动态规划

之所以叫伪动态规划, 是因为我不知道这算不算动态规划. 但是它确实利用了动态规划的思想. 利用之前已经计算过的信息.

我们从后向前计算, 首先最后一个肯定是0

对于i, 假设i之后的ans都已经被正确计算, 我们来计算ans[i]

首先取`j = i + 1`

如果`T[j] > T[i]`, 那么肯定有`ans[i] = j - i`

否则, 我们判断`ans[j]`. 如果`ans[j] = 0`, 就说明往后没有比`T[j]`更大的值了. 又因为`T[i] >= T[j]`所以`T[i]`肯定也等于0

如果`ans[j] != 0`, 那么令`tmp_j = ans[j]`. 说明第一个比T[j]大的数是`T[j + tmp_j]`. 那么第一个比T[i]大的数也只可能出现在j+j_tmp及其后面. 因为`T[i] >= T[j]`. 所以`j += ans[j]`, 继续寻找即可

在网上查的这种算法的时间复杂度也为O(n). 但我不会证明.

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] ans = new int[T.length];
        for(int i = T.length - 2; i > -1; --i)
        {
            int j = i + 1;
            while(j < T.length)
            {
                if(T[j] > T[i])
                {
                    ans[i] = j - i;
                    break;
                }
                if(ans[j] == 0)
                {
                    ans[i] = 0;
                    break;
                }
                else
                {
                    j += ans[j];
                }
            }
        }
        return ans;
    }
}
```

