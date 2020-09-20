The count-and-say sequence is the sequence of integers with the first five terms as following:

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2`, then `one 1"` or `1211`.

Given an integer *n* where 1 ≤ *n* ≤ 30, generate the *n*th term of the count-and-say sequence. You can do so recursively, in other words from the previous member read off the digits, counting the number of digits in groups of the same digit.

Note: Each term of the sequence of integers will be represented as a string.

## 1 迭代

先写一个read函数, 返回一个字符串的读音

然后调用read函数n - 1次, 最后返回结果. (但不知道为什么这么慢. faster than 5.04% of C++ online submissions for Count and Say.)

可能的改进点有, 把to_string函数换成强制类型转换, 先计算k = text.size()然后后面全部用k代替size()

```c++
class Solution {
public:
    string countAndSay(int n) {
        string ans = "1";
        while(--n)
        {
            ans = read(ans);
        }
        return ans;
    }
private:
    string read(string text)
    {
        string ans = "";
        int begin = 0;
        int end = 0;
        while(end < text.size())
        {
            while(end < text.size() && text[end] == text[begin])
                ++end;
            ans = ans + to_string(end - begin) + text[begin];
            begin = end;
        }
        return ans;
    }
};
```



