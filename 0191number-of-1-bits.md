Write a function that takes an unsigned integer and return the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

 

**Example 1:**

```
Input: 00000000000000000000000000001011
Output: 3
Explanation: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.
```

**Example 2:**

```
Input: 00000000000000000000000010000000
Output: 1
Explanation: The input binary string 00000000000000000000000010000000 has a total of one '1' bit.
```

**Example 3:**

```
Input: 11111111111111111111111111111101
Output: 31
Explanation: The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits.
```

 

**Note:**

- Note that in some languages such as Java, there is no unsigned integer type. In this case, the input will be given as signed integer type and should not affect your implementation, as the internal binary representation of the integer is the same whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two's_complement). Therefore, in **Example 3** above the input represents the signed integer `-3`.

## 1 位运算1

对于一个数n, 如果和1做位与运算, 结果是1, 那说明这个数的二进制表示末尾位为1, 反之为0.

再将这个数右移1位与1做位与运算, 计算出这个数的二进制表示倒数第2位是不是1, 以此类推....

直到把这个数右移成0.

要注意, 假如n为负数的话, n>>1会在首位补1, 导致死循环. 所以要用无符号右移 n>>>1

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int ans = 0;
        while(n != 0)
        {
            int tmp = n & 1;
            ans += tmp;
            n = n >>> 1;
        }
        return ans;
    }
}
```

## 2 位运算2

使用技巧, 不断更新`n = n & (n - 1)`. 直到n == 0. 中间更新的次数即为1的个数.

这个方法看上去难理解, 但是从纸上写一写就清楚了. 每做一次`n = n & (n - 1)`的操作, 1的总个数就会减1.

这两种位运算的方法在速度上没有区别.

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int ans = 0;
        while(n != 0)
        {
            n = n & (n - 1);
            ++ans;
        }
        return ans;
    }
}
```

