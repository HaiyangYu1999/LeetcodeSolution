Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero, which means losing its fractional part. For example, `truncate(8.345) = 8` and `truncate(-2.7335) = -2`.

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = truncate(3.33333..) = 3.
```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = truncate(-2.33333..) = -2.
```

**Note:**

- Both dividend and divisor will be 32-bit signed integers.
- The divisor will never be 0.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function **returns 231 − 1 when the division result overflows**.

## 1 暴力除法

首先记录下来结果的正负isNegative, 然后把dividend和divisor全部转为相应的负数. (至于为什么不转换成正数, 因为Integer.MIN_VALUE的绝对值是比Integer.MAX_VALUE大1的, 转成正数麻烦)

然后dividend不断地去减divisor(每减一次ans加1), 直到dividend > divisor为止. 再根据结果的正负添加ans的正负即可.

但是, 如果dividend很大, divisor很小, 非常慢!

```java
class Solution {
    public int divide(int dividend, int divisor) {
        if(divisor == 1) 
            return dividend;
        if(dividend == Integer.MIN_VALUE && divisor == -1)
            return Integer.MAX_VALUE;
        boolean isNegative = (dividend < 0) && (divisor > 0) || (dividend > 0) && (divisor < 0);
        dividend = (dividend < 0) ? dividend : -dividend;
        divisor = (divisor < 0) ? divisor : -divisor;
        int ans = 0;
        while(dividend <= divisor)
        {
            ++ans;
            dividend -= divisor;
        }
        return isNegative ? -ans : ans;
    }
}
```

## 2 位运算

**这题的位运算超难! 思路倒是不难, 难的是各种边界条件!**

首先要把所有数转换为正数, 负数的位运算更难!!!

`divisor_`和`dividend_`均为取正数的long类型, `abs_dividend`和`abs_divisor`同理(下面假设dividend和divisor均为正数)

然后将`divisor_`左移k位, 直到`divisor_`刚好大于`dividend_`.

此时, --k, 并且让`dividend_`除以`divisor_`, 得到一个商div, 说明`dividend_`里至少有`div * power(2,k)`个`divisor`. 所以更新`ans += div * power(2,k)` , 更新`dividend_ -= div * power(2,k) * divisor`. 依次循环, 直到`dividend_ < divisor`或者`divisor_ < divisor `.

`divisor_ < divisor ` 是保证`divisor_`在右移的过程中不要小于divisor.



举个例子说明

>dividend = 10 divisor = 3
>
>首先左移divisor直到大于10, 此时k = 2, `divisor_ = 12`
>
>然后右移`divisor_ = 6`, `k = 1`
>
>这时, 10 里面有1个6, `div = 1`, 所以 `ans += 1 * power(2, 1)`, `dividend_ -= 1 * power(2, 1) * 3` (ans = 2, dividend_ = 10 - 6 = 4)
>
>这时`dividend_`仍然大于等于divisor, 所以继续进入while
>
>右移`divisor_ = 3`, k = 0,
>
>这时, 4 里面有1个3, `div = 1`, 所以 `ans += 1 * power(2, 0)`, `dividend_ -= 1 * power(2, 0) * 3` (ans = 3, dividend_ = 4 - 3 = 1)
>
>此时, `dividend_ < divisor`, 返回ans为3.

```java
class Solution {
    public int divide(int dividend, int divisor) {
        if(divisor == 1)
            return dividend;
        if(dividend == Integer.MIN_VALUE && divisor == -1)
            return Integer.MAX_VALUE;
        boolean isNegative = (dividend < 0) && (divisor > 0) || (dividend > 0) && (divisor < 0);
        //use long to avoid overflow e.g. (-3 << 31) will be 1073741824
        long dividend_ = (dividend < 0) ? -(long)dividend : (long)dividend;
        long divisor_ = (divisor < 0) ? -(long)divisor : (long)divisor;
        long abs_dividend = dividend_;
        long abs_divisor = divisor_;
        if(dividend_ < divisor_)
            return 0;
        int ans = 0;

        int k = 0;
        while(divisor_ <= dividend_)
        {
            divisor_ = divisor_ << 1;
            ++k;
        }
        System.out.println(dividend_);
        System.out.println(divisor_);
        while(dividend_ >= abs_divisor && divisor_ >= abs_divisor)
        {
            divisor_ = divisor_ >> 1;
            --k;
            int div = div(dividend_, divisor_);
            ans += div << k;  //div * power(2,k);
            dividend_ -= multiply((div << k) , abs_divisor);
        }
        return isNegative ? -ans : ans;
    }
    private int div(long a, long b)
    {
        //a and b are both positive.
        if(a < b)
            return 0;
        else if(a == b || a < b + b)
            return 1;
        else
            return 2;
    }

    private long multiply(long a, long b)
    {
        long ans = 0;
        if(a < b)
        {
            long tmp = a;
            a = b;
            b = tmp;
        }
        for(long i = 0; i < b; ++i)
        {
            ans += a;
        }
        return ans;
    }
}
```

