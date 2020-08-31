Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

**Note:**
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## 常规解法

不断地求模获得每一位的数字储存起来, 再重组

```c++
class Solution {
public:
    int reverse(int x) {
        if(x == 0)
        {
            return x;
        }
        else 
        {
            bool isNegative = (x < 0);
            x = abs(x);
            queue<int> q;
            while(x > 0)
            {
                int tmp = x % 10;
                x = x / 10;
                q.push(tmp);
            }
            int result = 0;
            while(!q.empty())
            {
                int tmp = q.front();
                q.pop();
                if(result > INT_MAX / 10)
                    return 0;
                result = result * 10 + tmp;
            }
            return isNegative ? -result : result;
        }
    }
};
```

## 常规解法2 

其实不需要new一个queue, 直接在result上更改就可以了

```c++
class Solution {
public:
    int reverse(int x) {
        if(x == 0)
        {
            return x;
        }
        else 
        {
            bool isNegative = (x < 0);
            x = abs(x);
            int result = 0;
            while(x > 0)
            {
                int tmp = x % 10;
                x = x / 10;
                if(result > INT_MAX / 10) return 0;
                result = result * 10 + tmp;
            }
            return isNegative ? -result : result;
        }
    }
};
```



