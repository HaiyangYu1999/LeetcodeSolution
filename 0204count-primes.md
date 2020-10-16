Count the number of prime numbers less than a non-negative number, `n`.

 

**Example 1:**

```
Input: n = 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

**Example 2:**

```
Input: n = 0
Output: 0
```

**Example 3:**

```
Input: n = 1
Output: 0
```

 

**Constraints:**

- `0 <= n <= 5 * 106`

## 1 暴力(超时)

对于1到n-1这些数, 每一个数都判断一次. 时间复杂度O(n^1.5)

```java
class Solution {
    public int countPrimes(int n) {
        if(n == 0 || n == 1)
            return 0;
        int cnt = 0;
        for(int i = 1; i < n; ++i)
        {
            if(isPrime(i))
                ++cnt;
        }
        return cnt;
    }
    private boolean isPrime(int n)
    {
        if(n == 0 || n == 1)
            return false;
        if(n == 2)
            return true;
        for(int i = 2; i <= Math.sqrt(n); ++i)
        {
            if(n % i == 0)
                return false;
        }
        return true;
    }
}
```


## 2 打表

**埃拉托斯特尼筛法**. 

首先对于质数2, 把所有的2的倍数(4,6,8...)标记为非质数, 然后再把所有的3的倍数(6,9,12...)标记为非质数, 以此类推....

所有的没有被标记的数即为质数

```java
class Solution {
    public int countPrimes(int n) {
        if(n == 0 || n == 1)
            return 0;
        boolean[] isNotPrime = new boolean[n];
        isNotPrime[0] = isNotPrime[1] = true;
        int cnt = 0;
        for(int i = 2; i < n; ++i)
        {
            if(!isNotPrime[i])
            {
                ++cnt;
                for(int j = 2; i * j < n; ++j)
                {
                    isNotPrime[i * j] = true;
                }
            }
        }
        return cnt;
    }
}
```

