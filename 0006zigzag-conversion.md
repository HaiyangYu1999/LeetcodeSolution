The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows);
```

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

## 直接构造

考虑字符串"0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16"

n=5

那么构造出来的结果就是

> 0				8										16
>
> 1			7	9								15
>
> 2		6			10					14
>
> 3	5					11		13
>
> 4								12

发现, 第1行的字符总为 `k * (2 * n - 2)`. k= 0, 1, 2, 3...这些位置的字符, 例如上面例子中, n = 5, 那么第一行所有的字符就为 0 8 16. 把这些先pushback到一个空的字符串s中.

第`i`, `(0 < i < n)`行中, 对应的元素总为 `k * (2 * n - 2) + i` 和  `(k + 1) * (2 * n - 2) - i`. 例如上面例子中的(1,7), (9,15). 然后对于每个i, 把这些字符pushback到s中.

最后, 第n-1行的元素总是为 `k * (2 * n - 2) + n - 1`.  k= 0, 1, 2, 3.... 例如上面例子中的4 12. push到s中即可.

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        const int& n = numRows;
        if(n == 1) return s;
        int len = s.size();
        string ans;
        for(int i = 0; i < n; ++i)
        {
            if(i == 0)
            {
                int k = 0;
                while(k * (2 * n - 2) < len)
                {
                    ans.push_back(s[k * (2 * n - 2)]);
                    ++k;
                }
            }
            else if(i == n - 1)
            {
                int k = 0;
                while(k * (2 * n - 2) + i < len)
                {
                    ans.push_back(s[k * (2 * n - 2) + i]);
                    ++k;
                }
            }
            else
            {
                
                int k = 0;
                while(true)
                {
                    if(k*(2*n-2) + i < len)
                        ans.push_back(s[k*(2*n-2) + i]);
                    else
                        break;
                    if((k+1)*(2*n-2) - i < len)
                        ans.push_back(s[(k+1)*(2*n-2) - i]);
                    else
                        break;
                    ++k;
                }
            }
        }
        return ans;
    }
};
```

