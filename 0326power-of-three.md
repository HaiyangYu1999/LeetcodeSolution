Given an integer, write a function to determine if it is a power of three.

**Example 1:**

```
Input: 27
Output: true
```

**Example 2:**

```
Input: 0
Output: false
```

**Example 3:**

```
Input: 9
Output: true
```

**Example 4:**

```
Input: 45
Output: false
```

**Follow up:**
Could you do it without using any loop / recursion?

## 1 暴力

对于所有的小于n的3的幂, 挨个试一试.

注意, 为了防止溢出, 要把a设置为long类型

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        long a = 1;
        while(a <= n)
        {
            if(a == n)
                return true;
            a *= 3;
        }
        return false;
    }
}
```

## 2 网上的答案

看了几个网上的答案, 感觉也都不是太好.

有利用`Integer.toString(number, base)`转化为3进制然后再用正则表达式判断字符串是不是`100000...000`这样的. 但问题是这个方法里面也用到了循环.

也有直接取对数的. 看看log_{3}(n)是不是整数. 但是double转int有可能会损失精度. 除非设置一个epsilon.

最好的方法是这种, [https://leetcode-cn.com/problems/power-of-three/solution/3de-mi-by-leetcode/](https://leetcode-cn.com/problems/power-of-three/solution/3de-mi-by-leetcode/)

因为n最大就是INT_MAX, 而且还有一半的负数, 所以这些数里面最大的就是3^19. 

用3^19来模n, 如果余数为0, 证明n是3的幂 