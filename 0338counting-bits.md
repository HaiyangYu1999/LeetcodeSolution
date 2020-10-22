Given a non negative integer number **num**. For every numbers **i** in the range **0 ≤ i ≤ num** calculate the number of 1's in their binary representation and return them as an array.

**Example 1:**

```
Input: 2
Output: [0,1,1]
```

**Example 2:**

```
Input: 5
Output: [0,1,1,2,1,2]
```

**Follow up:**

- It is very easy to come up with a solution with run time **O(n\*sizeof(integer))**. But can you do it in linear time **O(n)** /possibly in a single pass?
- Space complexity should be **O(n)**.
- Can you do it like a boss? Do it without using any builtin function like **__builtin_popcount** in c++ or in any other language.

## 1 计算末尾1的个数

首先, 如果对于一个数i, 末尾没有1, 那么i+1的1的个数就为i中1的个数+1. 例如 1110 -> 1111

如果i末尾有连续的j个1, 那么i+1就要将这个j个1都进位成0, 并且在第j+1位上置为1. 所以会比i的1个个数少j - 1个. 例如, 末尾有3个连续的1, 10111 -> 11000. 那么i+1就会比i中1的个数少3 - 1个

对于这种的时间复杂度, 主要集中在计算末尾1的个数.

我们设末尾有0个1, 需要花费1个时间.

末尾有1个1, 需要花费2个时间

.....以此类推, 有n-1个1需要花费n个时间.

而从0到n中, 末尾有0个1的约有n/2个, 末尾有1个1的约为n/4个, ..., 末尾有n-1个1的约有n/2^(n)个

所以总时间为 Σ_{n = 1}^{∞} n * (1/2)^n

复杂度约为O(2n)

```java
class Solution {
    public int[] countBits(int num) {
        int n = num;
        int[] ans = new int[n+1];
        ans[0] = 0;
        for(int i = 1; i <= n; ++i)
        {
            int iprev1 = getSuffix1(i - 1);
            ans[i] = (iprev1 == 0) ? (ans[i-1] + 1) : (ans[i-1] - iprev1 + 1);
        }
        return ans;
    }
    private int getSuffix1(int i)
    {
        int cnt = 0;
        while((i & 1) == 1)
        {
            ++cnt;
            i = i >>> 1;
        }
        return cnt;
    }
}
```

## 2 除2

这个方法更简单, 但我没想到. 唉, 还是菜

假如i是偶数, 那么i中1的个数和i/2中1的个数肯定一样,

假如i是奇数, 那么i中1的个数等于i/2中1的个数加1.

```java
class Solution {
    public int[] countBits(int num) {
        int n = num;
        int[] ans = new int[n+1];
        ans[0] = 0;
        if(n == 0)
            return ans;
        ans[1] = 1;
        for(int i = 2; i <= n; ++i)
        {
            ans[i] = ((i & 1) == 0) ? ans[i >>> 1] : ans[i >>> 1] + 1;
        }
        return ans;
    }
}
```

