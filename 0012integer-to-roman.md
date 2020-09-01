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

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: 3
Output: "III"
```

**Example 2:**

```
Input: 4
Output: "IV"
```

**Example 3:**

```
Input: 9
Output: "IX"
```

**Example 4:**

```
Input: 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```

**Example 5:**

```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## 1 🧠短路的做法

**至今仍然想不明白为什么我不用数组存储这个映射, 而是用hashmap...... 代码又慢又长...或许需要重新new一个🧠了**

```c++
class Solution {
public:
    string intToRoman(int num) {
        int one = num % 10;
        int ten = (num / 10) % 10;
        int hundred = (num / 100) % 10;
        int thousand = (num / 1000) % 10;
        return thousandDigit[thousand] + hundredDigit[hundred] + tenDigit[ten] + oneDigit[one];
    }
    Solution()
    {
        init();
    }
private:
    unordered_map<int, string> thousandDigit;
    unordered_map<int, string> hundredDigit;
    unordered_map<int, string> tenDigit;
    unordered_map<int, string> oneDigit;
    void init()
    {
        thousandDigit.insert({0,""});
        thousandDigit.insert({1,"M"});
        thousandDigit.insert({2,"MM"});
        thousandDigit.insert({3,"MMM"});
        hundredDigit.insert({0,""});
        hundredDigit.insert({1,"C"});
        hundredDigit.insert({2,"CC"});
        hundredDigit.insert({3,"CCC"});
        hundredDigit.insert({4,"CD"});
        hundredDigit.insert({5,"D"});
        hundredDigit.insert({6,"DC"});
        hundredDigit.insert({7,"DCC"});
        hundredDigit.insert({8,"DCCC"});
        hundredDigit.insert({9,"CM"});
        tenDigit.insert({0,""});
        tenDigit.insert({1,"X"});
        tenDigit.insert({2,"XX"});
        tenDigit.insert({3,"XXX"});
        tenDigit.insert({4,"XL"});
        tenDigit.insert({5,"L"});
        tenDigit.insert({6,"LX"});
        tenDigit.insert({7,"LXX"});
        tenDigit.insert({8,"LXXX"});
        tenDigit.insert({9,"XC"});
        oneDigit.insert({0,""});
        oneDigit.insert({1,"I"});
        oneDigit.insert({2,"II"});
        oneDigit.insert({3,"III"});
        oneDigit.insert({4,"IV"});
        oneDigit.insert({5,"V"});
        oneDigit.insert({6,"VI"});
        oneDigit.insert({7,"VII"});
        oneDigit.insert({8,"VIII"});
        oneDigit.insert({9,"IX"});
    }
};
```

## 2 正确做法

```c++
class Solution {
public:
    string intToRoman(int num) {
        int one = num % 10;
        int ten = (num / 10) % 10;
        int hundred = (num / 100) % 10;
        int thousand = (num / 1000) % 10;
        return thousandDigit[thousand] + hundredDigit[hundred] + tenDigit[ten] + oneDigit[one];
    }
private:
    string thousandDigit[4] = {"", "M", "MM", "MMM"};
    string hundredDigit[10] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
    string tenDigit[10] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
    string oneDigit[10] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
};
```

