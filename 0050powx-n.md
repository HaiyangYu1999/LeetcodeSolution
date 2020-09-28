Implement [pow(*x*, *n*)](http://www.cplusplus.com/reference/valarray/pow/), which calculates *x* raised to the power *n* (i.e. xn).

 

**Example 1:**

```
Input: x = 2.00000, n = 10
Output: 1024.00000
```

**Example 2:**

```
Input: x = 2.10000, n = 3
Output: 9.26100
```

**Example 3:**

```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

 

**Constraints:**

- `-100.0 < x < 100.0`
- `-231 <= n <= 231-1`
- `-104 <= xn <= 104`

## 1 快速幂

对于pow(x, n), 可以先计算出tmp = pow(x, n/2)然后再用tmp * tmp 或 tmp * x * tmp表示出power(x,n).

要注意n为Integer.MIN_VALUE的时候, 转成正数会overflow. 所以构造一个辅助函数, `double myPowLong(double x, long n)`解决这个问题.

 ```java
class Solution {
    public double myPow(double x, int n) {
        return myPowLong(x, (long)n);
    }
    private double myPowLong(double x, long n)
    {
        if(n == 0)
            return 1;
        else if(x == 1.0)
            return x;
        else if(n < 0)
            return 1/myPowLong(x,-n);
        double tmp = myPowLong(x,n/2);
        if(n % 2 == 0)
        {
            return tmp * tmp;
        }
        else
            return tmp * x * tmp;
    }
}
 ```

## 2 快速幂 非递归方法

在递归方法中, 需要O(logn)的空间复杂度.

对于迭代方法, 首先将n做二进制分解, 然后根据每一位的值决定是否乘一个多余的x.

例如 n = 9, 二进制表示为1001. 

所以$x^n = (x^{8})^1 * (x^{4})^0 * (x^{2})^0 * (x^1)^1$  

首先二进制n的最后一位为1, x = x0, ans = x0,

然后更新n >>= 1 和x *=x. 更新完之后n的最后一位为0, x = x0^2

所以**不需要**进行ans *= x的操作 ans还是x0. 继续更新 n和x, 更新完之后n的最后一位还是为0, x = x0^4

这时仍然不需要更新ans. 继续更新 n和x, 更新完之后n的最后一位是1, x = x0^8

所以ans *= x, 这时ans = x0^9. 继续更新n和x, n更新完了之后为0了, 退出循环. 得到正确结果ans = x0^9

```java
class Solution {
    public double myPow(double x, int n) {
        return myPowLong(x, (long)n);
    }
    private double myPowLong(double x, long n)
    {
        if(n == 0)
            return 1;
        else if(x == 1.0)
            return x;
        else if(n < 0)
            return 1/myPowLong(x,-n);
        double ans = 1.0;
        while(n > 0)
        {
            long lastDigit = n & 1;
            if(lastDigit == 1) ans *= x;
            x *= x;
            n = n >> 1;
        }
        return ans;
    }
}
```

