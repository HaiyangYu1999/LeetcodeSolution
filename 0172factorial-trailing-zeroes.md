Given an integer `n`, return *the number of trailing zeroes in `n!`*.

**Follow up:** Could you write a solution that works in logarithmic time complexity?

 

**Example 1:**

```
Input: n = 3
Output: 0
Explanation: 3! = 6, no trailing zero.
```

**Example 2:**

```
Input: n = 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```

**Example 3:**

```
Input: n = 0
Output: 0
```

 

**Constraints:**

- `1 <= n <= 104`

## 数学方法

首先n!中0的个数为含有5的因子的个数. 

例如, 10!里面有5和10两个数含有因子5, 所以是2.

25!中, 5, 10, 15, 20, 25都含有因子5, 但是25含有2个因子5, 所以这里有6个因子5. 以此类推125含有3个5, 625含有4个因子5...

所以, 我们先从(5, 10, 15,20, 25, ..)这些数中取出1个因子5来(一共n/5个数), 然后将 n 除以 5, 这样, 25就变成了5, 即含有多个因子5的数还能再被除一次.以此类推直到n == 0

```java
class Solution {
    public int trailingZeroes(int n) {
        int ans = 0;
        while(n > 0)
        {
            n /= 5;
            ans += n;
        }
        return ans;
    }
}
```

