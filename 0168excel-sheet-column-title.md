Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
```

**Example 1:**

```
Input: 1
Output: "A"
```

**Example 2:**

```
Input: 28
Output: "AB"
```

**Example 3:**

```
Input: 701
Output: "ZY"
```

## 特殊的26进制

这题想了好几天才想明白.

这个26进制是特殊的. 没有0, 但是有26.

首先要计算普通的26进制表示, 有0没26的(在这个题里就是有'@'没有'Z')

例如 `457002 -> 26^4 + 26^1-> "A@@A@"`

**然后从后向前地借位,** 如果发现某一位是0或者比0小(这里的0就是'@'), 就从前面的位置借一位

比如, "A@@A@"最后一位就是0, 不满足要求, 从前面借一位变为'@' + 26, 即'Z'. 而前面的一位变为A - 1 = @

`"A@@A@" -> "A@@@Z"`

倒数第二位现在也不符合要求了, 再借一位变为@ + 26 = Z, 而前面的倒数第三位变为了@ - 1 = ?

`"A@@@Z" -> "A@?ZZ"`

倒数第三位再从倒数第二位借位, 变为 ? + 26 = Y, 倒数第四位变为 @ - 1 = ?

`"A@?ZZ" -> "A?YZZ"`

最后, 倒数第四位再向第一位借位, 变为 ? + 26 = Y, 第一位变为 A - 1 = @

`"A?YZZ" -> "@YYZZ"`

最后去掉前面的所有0, (所有'@')即可

`"@YYZZ" -> "YYZZ"`

```c++
class Solution {
public:
    string convertToTitle(int n) {
        string s = "";
        while(n > 0)
        {
            int tmp = n % 26;
            s.push_back(tmp + '@'); // '@' equals 'A' - 1. 
            n /= 26;
        }
        s = string(s.rbegin(), s.rend());
        for(int i = s.size() - 1; i > 0; --i)
        {
            if(s[i] <= '@')
            {
                s[i] += 26;
                s[i-1] -= 1;
            }
        }
        return string(std::find_if(s.begin(), s.end(), [](char ch){return ch != '@';}), s.end());
    }
};
```

