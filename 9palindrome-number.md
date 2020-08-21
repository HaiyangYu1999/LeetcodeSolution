Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Example 1:**

```
Input: 121
Output: true
```

**Example 2:**

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

**Follow up:**

Coud you solve it without converting the integer to a string?

## 1 队列

既然不让转化成字符串, 那就用队列吧, 慢点归慢点, 实在想不到更好的办法了, 要防止数组倒置后超出INT范围, 所以要加判断`x1 < INT_MAX / 10`

```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return false;
        int x0 = x;
        queue<int> q;
        do
        {
            int tmp = x0 % 10;
            q.push(tmp);
            x0 /= 10;
        }while(x0 > 0);
        int x1 = 0;
        while(!q.empty() && x1 < INT_MAX / 10)
        {
            x1 = x1 * 10 + q.front();
            q.pop();
        }
        return x1 == x;
    }
};
```

## 2 方法2

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/palindrome-number/solution/hui-wen-shu-by-leetcode-solution/

映入脑海的第一个想法是将数字转换为字符串，并检查字符串是否为回文。但是，这需要额外的非常量空间来创建问题描述中所不允许的字符串。

第二个想法是将数字本身反转，然后将反转后的数字与原始数字进行比较，如果它们是相同的，那么这个数字就是回文。
但是，如果反转后的数字大于 \text{int.MAX}int.MAX，我们将遇到整数溢出问题。

按照第二个想法，为了避免数字反转可能导致的溢出问题，为什么不考虑只反转 数字的一半？毕竟，如果该数字是回文，其后半部分反转后应该与原始数字的前半部分相同。

例如，输入 1221，我们可以将数字 “1221” 的后半部分从 “21” 反转为 “12”，并将其与前半部分 “12” 进行比较，因为二者相同，我们得知数字 1221 是回文。

算法

首先，我们应该处理一些临界情况。所有负数都不可能是回文，例如：-123 不是回文，因为 - 不等于 3。所以我们可以对所有负数返回 false。除了 0 以外，所有个位是 0 的数字不可能是回文，因为最高位不等于 0。所以我们可以对所有大于 0 且个位是 0 的数字返回 false。

现在，让我们来考虑如何反转后半部分的数字。

对于数字 1221，如果执行 1221 % 10，我们将得到最后一位数字 1，要得到倒数第二位数字，我们可以先通过除以 10 把最后一位数字从 1221 中移除，1221 / 10 = 122，再求出上一步结果除以 10 的余数，122 % 10 = 2，就可以得到倒数第二位数字。如果我们把最后一位数字乘以 10，再加上倒数第二位数字，1 * 10 + 2 = 12，就得到了我们想要的反转后的数字。如果继续这个过程，我们将得到更多位数的反转数字。

现在的问题是，我们如何知道反转数字的位数已经达到原始数字位数的一半？

由于整个过程我们不断将原始数字除以 10，然后给反转后的数字乘上 10，所以，当原始数字小于或等于反转后的数字时，就意味着我们已经处理了一半位数的数字了。



```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x >= 0 && x < 10) 
            return true;
        if(x < 0 || x % 10 == 0)
            return false;
        int x1 = 0;
        while(x > x1)
        {
            int tmp = x % 10;
            x /= 10;
            x1 = x1 * 10 + tmp;
            if(x1 == x || x1 == x / 10)
                return true;
        }
        return false;
    }
};
```

