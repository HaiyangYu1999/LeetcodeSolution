Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Example 1:**

```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

**Note:**

1. The length of both `num1` and `num2` is < 110.
2. Both `num1` and `num2` contain only digits `0-9`.
3. Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

## 1 常规解法

先实现两个字符串相加的函数`add()`, 然后实现一个字符串与一个0-9的数相乘的函数`simpleMultiply()`. 最后通过这两个函数实现`multiply()`. 过程清晰但是复杂. 复杂度是O(2mn), n次m长的字符串与整数相乘有m\*n的复杂度, m次相加有m\*n的复杂度.

```c++
class Solution {
public:
    string multiply(string num1, string num2) {
        string answer = "0";
        for(int i = 0; i < num2.size(); ++i)
        {
            string simpleProduct = simpleMultiply(num1, num2[i] - '0') + string(num2.size() - i - 1, '0');
            answer = add(answer, simpleProduct);
        }
        int k = 0;
        while(answer[k] == '0' && k < answer.size() - 1)
            ++k;
        return string(answer.begin() + k, answer.end());
    }
private:
    string add(string num1, string num2)
    {
        stack<char> result;
        int len = max(num1.size(), num2.size());
        if(num1.size() < len)
        {
            num1 = string(len - num1.size(),'0') + num1;
        }
        else
        {
            num2 = string(len - num2.size(),'0') + num2;
        }

        int carry = 0;
        for(int i = len - 1; i >= 0; --i)
        {
            int sum = (num1[i] - '0') + (num2[i] - '0') + carry;
            carry = sum / 10;
            sum %= 10;
            result.push(sum + '0');
        }
        if(carry > 0)
        {
            result.push(carry + '0');
        }
        string ans;
        while(!result.empty())
        {
            ans.push_back(result.top());
            result.pop();
        }
        return ans;
    }
    string simpleMultiply(string num1, int num2)
    {
        assert(num2 >= 0 && num2 <= 9);
        int len = num1.size();
        stack<char> result;
        int carry = 0;
        for(int i = len - 1; i >= 0; --i)
        {
            int product = (num1[i] - '0') * num2 + carry;
            carry = product / 10;
            product %= 10;
            result.push(product + '0');
        }
        if(carry > 0)
        {
            result.push(carry + '0');
        }
        string ans;
        while(!result.empty())
        {
            ans.push_back(result.top());
            result.pop();
        }
        return ans;
    }
};
```

## 2 改进常规解法

新开辟一个长度为num1.size() + num2.size()的数组

**观察到, num1的第i位与num2的第j位的乘积一定是在结果的第i + j + 1 位, 如果有进位则是进位在i + j位**

例如 "123" 和 "456"中第2位(个位)的乘积在第5位(新开辟数组的最后一位).

依次遍历i,j, 然后往结果里面填值即可.

这个方法复杂度低, 但是较为容易出错.

```c++
class Solution {
public:
   string multiply(string num1, string num2) {
        if(num1 == "0" || num2 == "0")
            return "0";
        vector<int> ans(num1.size() + num2.size());
        for(int i = num1.size() - 1; i >= 0; --i)
        {
            for(int j = num2.size() - 1; j >= 0; --j)
            {
                int prod = (num1[i] - '0') * (num2[j] - '0') + ans[i + j + 1];
                ans[i + j] += prod / 10;
                ans[i + j + 1] = prod % 10;
            }
        }
        for(auto& i : ans)
            i += '0'; // int to char
        string s(ans.begin(), ans.end());
        int k = 0;
        while(s[k] == '0')
            ++k;
        return string(s.begin() + k, s.end());
    }
};
```

