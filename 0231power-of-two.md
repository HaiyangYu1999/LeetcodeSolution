Given an integer `n`, write a function to determine if it is a power of two.

 

**Example 1:**

```
Input: n = 1
Output: true
Explanation: 20 = 1
```

**Example 2:**

```
Input: n = 16
Output: true
Explanation: 24 = 16
```

**Example 3:**

```
Input: n = 3
Output: false
```

**Example 4:**

```
Input: n = 4
Output: true
```

**Example 5:**

```
Input: n = 5
Output: false
```

 

**Constraints:**

- `-231 <= n <= 231 - 1`

## 位运算

直接判断二进制的n 是不是 **开头一个1, 后面n个0**的形式

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n <= 0)
            return false;
        while(n != 1)
        {
            if((n & 1) == 0)
            {
                n = n >>> 1;
            }
            else
            {
                if(n != 1)
                    return false;
            }
        }
        return true;
    }
}
```

