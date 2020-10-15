Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:

```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

**Example 1:**

```
Input: "A"
Output: 1
```

**Example 2:**

```
Input: "AB"
Output: 28
```

**Example 3:**

```
Input: "ZY"
Output: 701
```

 

**Constraints:**

- `1 <= s.length <= 7`
- `s` consists only of uppercase English letters.
- `s` is between "A" and "FXSHRXW".

## 26进制

本质上就是26进制. 只不过这种方式没法表示0. 因为是从1开始的

所以要注意计算的时候有`int tmp = s.charAt(i) - 'A' + 1;` 而不是`int tmp = s.charAt(i) - 'A';`

```java
class Solution {
    public int titleToNumber(String s) {
        if(s == null || "".equals(s))
            return 0;
        int ans = 0;
        for(int i = 0; i < s.length(); ++i)
        {
            ans *= 26;
            if(s.charAt(i) < 'A' || s.charAt(i) > 'Z'){
                throw new RuntimeException("string has non-uppercase char.");
            }
            int tmp = s.charAt(i) - 'A' + 1;
            ans += tmp;
        }
        return ans;
    }
}
```

