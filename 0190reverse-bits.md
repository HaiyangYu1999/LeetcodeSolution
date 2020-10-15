Reverse bits of a given 32 bits unsigned integer.

**Note:**

- Note that in some languages such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two's_complement). Therefore, in **Example 2** above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.

**Follow up**:

If this function is called many times, how would you optimize it?

 

**Example 1:**

```
Input: n = 00000010100101000001111010011100
Output:    964176192 (00111001011110000010100101000000)
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.
```

**Example 2:**

```
Input: n = 11111111111111111111111111111101
Output:   3221225471 (10111111111111111111111111111111)
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.
```

 

**Constraints:**

- The input must be a **binary string** of length `32`

## 1 暴力

对于i从 0 到 31, j = 31 - i, 我们需要交换第i位和第j位. 

首先求出第i位和第j位的值a和b. 然后设置第j位为a, 设置第i位为b.重复16次这样的操作.

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        for(int i = 0; i < 16; ++i)
        {
            int j = 31 - i;
            int a = ((1 << i) & n) >>> i;
            int b = ((1 << j) & n) >>> j;
            n = (a == 0) ? (n & ~(1 << j)) : (n | (1 << j));
            n = (b == 0) ? (n & ~(1 << i)) : (n | (1 << i));
        }
        return n;
    }
}
```

## 2 分治

首先要注意java里面一定要用无符号右移!!! 不然负数的右移会出现最左侧补1的错误而不是最左侧补0. 

首先先交换前16位和后16位. 即 `n = (n >>> 16) | (n << 16)`

然后在前16位中, 交换前8位和后8位, 后16位中, 也是交换前8位和后8位.

具体的做法如下

> **首先定义两个数, 可以把前8位的元素和后8位的元素提取出来.**
>
> a1 = 0000 0000 1111 1111 0000 0000 1111 1111
>
> a2 = 1111 1111 0000 0000 1111 1111 0000 0000
>
> 这样, n & a1就是后8位的元素, n & a2就是前8位的元素. 然后交换. 
>
> `n = ((n & a1) << 8) | ((n & a2) >>> 8)`

然后每8位一组, 交换前4个元素和后4个元素.

> 此时的a1和a2应该为:
>
> a1 = 0000 1111 0000 1111 0000 1111 0000 1111
>
> a2 = 1111 0000 1111 0000 1111 0000 1111 0000
>
> `n = ((n & a1) << 4) | ((n & a2) >>> 4)`

然后每4位一组, 交换前2个元素和后2个元素

> 此时的a1和a2应该为:
>
> a1 = 0011 0011 0011 0011 0011 0011 0011 0011
>
> a2 = 1100 1100 1100 1100 1100 1100 1100 1100
>
> `n = ((n & a1) << 2) | ((n & a2) >>> 2)`

然后每2位一组, 交换前1个元素和后1个元素

> 此时的a1和a2应该为:
>
> a1 = 0101 0101 0101 0101 0101 0101 0101 0101
>
> a2 = 1010 1010 1010 1010 1010 1010 1010 1010
>
> `n = ((n & a1) << 1) | ((n & a2) >>> 1)`

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        n = (n >>> 16) | (n << 16);
        int a1 = 0b00000000111111110000000011111111;
        int a2 = 0b11111111000000001111111100000000;
        n = ((n & a1) << 8) | ((n & a2) >>> 8);
        a1 = 0b00001111000011110000111100001111;
        a2 = 0b11110000111100001111000011110000;
        n = ((n & a1) << 4) | ((n & a2) >>> 4);
        a1 = 0b00110011001100110011001100110011;
        a2 = 0b11001100110011001100110011001100;
        n = ((n & a1) << 2) | ((n & a2) >>> 2);
        a1 = 0b01010101010101010101010101010101;
        a2 = 0b10101010101010101010101010101010;
        n = ((n & a1) << 1) | ((n & a2) >>> 1);
        return n;
    }
}
```

