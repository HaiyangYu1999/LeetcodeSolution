Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

**Note:**

- Only the space character `' '` is considered as whitespace character.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.

**Example 1:**

```
Input: "42"
Output: 42
```

**Example 2:**

```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```

**Example 3:**

```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

**Example 4:**

```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```

**Example 5:**

```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.
```

## 从左到右扫描

首先先去除左边的空格

```c++
str = string(std::find_if(str.begin(), str.end(),[](char ch){
            return !std::isspace(ch);
        }), str.end());
```

然后检查第0个字符是不是'+', '-',数字. 如果不是直接返回0

然后判断正负, 储存在变量isNegative中

接下来就是创建一个变量ans, 读一个字母储存一次`ans = ans * 10 + tmp` 其中, `tmp = str[i] - '0'`.

假如读到的不是digit, 返回ans即可

**上面的步骤看起来并不算难, 但难的地方在于处理INT_MIN, INT_MAX**

在更新ans前, 要检查更新后是不是会overflow. 

首先检查ans, 如果是正数, 并且ans > INT_MAX/10, 因为我们这次读到的tmp也是数字, 如果更新`ans = ans * 10 + tmp`的话肯定会overflow! 所以这种情况就直接返回INT_MAX即可.

如果检查发现 ans == INT_MAX/10, 并且tmp >= 7, 因为INT_MAX = 2147483647. 所以更新ans后要么新的值为INT_MAX, 要么就超过INT_MAX, 所以这两种情况都返回INT_MAX也是没有错误的.

负数同理, 不过要注意负数的INT_MIN = -2147483648 所以如果ans == INT_MAX/10, 并且tmp >= 8, 才返回INT_MIN.

```c++
//保证更新tmp后不会溢出
if(!isNegative)
{
    if(ans > INT_MAX/10 || (ans == INT_MAX/10 && tmp >= 7))
        return INT_MAX;
}
else
{
    if(ans > INT_MAX/10 || (ans == INT_MAX/10 && tmp >= 8))
        return INT_MIN;
}
ans = ans * 10 + tmp;
```

```c++
class Solution {
public:
    int myAtoi(string str) {
        //trim left space
        str = string(std::find_if(str.begin(), str.end(),[](char ch){
            return !std::isspace(ch);
        }), str.end());
        if(!isSign(str[0]) && !std::isdigit(str[0]))
            return 0;
        bool isNegative = (str[0] == '-');
        int ans = 0;
        for(int i = 0; i < str.size(); ++i)
        {
            if(i == 0 && isSign(str[i]))
                continue;
            if(!std::isdigit(str[i]))
                return isNegative ? -ans : ans;
            int tmp = str[i] - '0';
            if(!isNegative)
            {
                if(ans > INT_MAX/10 || (ans == INT_MAX/10 && tmp >= 7))
                    return INT_MAX;
            }
            else
            {
                if(ans > INT_MAX/10 || (ans == INT_MAX/10 && tmp >= 8))
                    return INT_MIN;
            }
            ans = ans * 10 + tmp;
        }
        return isNegative ? -ans : ans;
    }
private:
    bool isSign(char ch)
    {
        return ch == '-' || ch == '+';
    }
};
```



