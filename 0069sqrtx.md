Implement `int sqrt(int x)`.

Compute and return the square root of *x*, where *x* is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

**Example 1:**

```
Input: 4
Output: 2
```

**Example 2:**

```
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
```

## 二分查找

将问题转化为, 从0到x中查找最后一个mid使得 mid * mid <= x的元素即可.

同时, 条件中要写`mid <= x / mid` 而不是`mid * mid <= x`, 否则mid*mid会溢出

```java
class Solution {
    public int mySqrt(int x) {
        if(x == 0 || x == 1) return x;
        int begin = 0;
        int end = x;
        while(begin <= end)
        {
            int mid = begin + (end - begin) / 2;
            if(mid <= x / mid)
            {
                begin = mid + 1;
            }
            else
            {
                end = mid - 1;
            }
        }
        return begin - 1;
    }
}
```

