Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: "III"
Output: 3
```

**Example 2:**

```
Input: "IV"
Output: 4
```

**Example 3:**

```
Input: "IX"
Output: 9
```

**Example 4:**

```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 5:**

```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## 一遍扫描

先构建一个字符到整数的映射, 扫描的时候**碰到某个字符就加或减其对应的值**

关键是如何确定加还是减

发现, 只需要比较第i位的字符和第i+1位的字符大小即可

> 例如, IX中, I < X, 所以I为-1而不是1.
>
> MCMXCIV 中, 第一个C后面是M, 而C < M, 这时C就代表-100, 第二个C后面是I, C > I, 这时C就代表100.

```c++
class Solution {
public:
    int romanToInt(string s) {
        int ans = 0;
        for(int i = 0; i < s.size(); ++i)
        {
            if(i == s.size() - 1)
            {
                ans += charToInt[s[i]];
            }
            else
            {
                bool isNegative = (charToInt[s[i]] < charToInt[s[i+1]]);
                ans += isNegative ? -charToInt[s[i]] : charToInt[s[i]];
            }
        }
        return ans;
    }
    Solution()
    {
        init();
    }
private:
    unordered_map<char, int> charToInt;
    void init()
    {
        charToInt.insert({'I',1});
        charToInt.insert({'V',5});
        charToInt.insert({'X',10});
        charToInt.insert({'L',50});
        charToInt.insert({'C',100});
        charToInt.insert({'D',500});
        charToInt.insert({'M',1000});
    }
};
```



