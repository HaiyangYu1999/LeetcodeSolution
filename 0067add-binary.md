Given two binary strings, return their sum (also a binary string).

The input strings are both **non-empty** and contains only characters `1` or `0`.

**Example 1:**

```
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```
Input: a = "1010", b = "1011"
Output: "10101"
```

 

**Constraints:**

- Each string consists only of `'0'` or `'1'` characters.
- `1 <= a.length, b.length <= 10^4`
- Each string is either `"0"` or doesn't contain any leading zero.

## 进位加法

仿照Leetcode 43 中的进位加法即可.

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        vector<int> ans;
        int len = max(a.size(), b.size());
        if(a.size() < len)
        {
            a = string(len - a.size(), '0') + a;
        }
        else
        {
            b = string(len - b.size(), '0') + b;
        }
        int carry = 0;
        for(int i = len - 1; i >= 0; --i)
        {
            int sum = (a[i] - '0') + (b[i] - '0') + carry;
            carry = sum / 2;
            ans.push_back(sum % 2);
        }
        if(carry)
            ans.push_back(carry);
        for(auto& i: ans)
            i += '0';
        return string(ans.rbegin(), ans.rend());
    }
};
```

