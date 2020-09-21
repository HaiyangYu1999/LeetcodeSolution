Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note:** For the purpose of this problem, we define empty string as valid palindrome.

**Example 1:**

```
Input: "A man, a plan, a canal: Panama"
Output: true
```

**Example 2:**

```
Input: "race a car"
Output: false
```

 

**Constraints:**

- `s` consists only of printable ASCII characters.

## 1 构造新的字符串

先构造一个新的字符串tmp, 储存原字符串删掉非alpha num类型的字符并且全变为小写的字符. 然后在判断tmp是不是palindrome.

但是这需要额外的空间复杂度

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        string tmp;
        for(char i : s)
        {
            if(std::isalnum(i))
                tmp.push_back(tolower(i));
        }
        
        for(int i = 0; i < tmp.size() / 2; ++i)
        {
            if(tmp[i] != tmp[tmp.size() - 1 - i])
                return false;
        }
        return true;
    }
};
```

## 2 在原字符串上直接判断

思路是, 使用两个指针, left和right. 每一次比较left和right指向的值是否相等. 当left >= right时停止迭代.

更新left和right的时候, 跳过非字母和数字的字符. 

但是要注意没有任何字母数字的字符串的判断 `",:"`这样的

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0;
        int right = s.size() - 1;
        while(left < right)
        {
            while(left < s.size() - 1 && !std::isalnum(s[left]))
                ++left;
            while(right > 0 && !std::isalnum(s[right]))
                --right;
            if(left == s.size() - 1)  //make string likes this ",:" true
                return true;
            if(std::tolower(s[left]) == std::tolower(s[right]))
            {
                ++left;
                --right;
            }
            else
                return false;
        }
        return true;
    }
};
```

