Given a string *s* consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word (last word means the last appearing word if we loop from left to right) in the string.

If the last word does not exist, return 0.

**Note:** A word is defined as a **maximal substring** consisting of non-space characters only.

**Example:**

```
Input: "Hello World"
Output: 5
```

## 模拟

先trim掉最后面的空格, 然后从后向前计数, 碰到空格停止计数, 最后返回计数的值即可

注意, 这里去除掉最后的空格的函数写法

`s.erase(find_if(s.rbegin(), s.rend(), [](char ch){return !std::isspace(ch);}).base(), s.end());`.

其中`find_if(s.rbegin(), s.rend(), [](char ch){return !std::isspace(ch);})`利用反向迭代器寻找最后一个不是空格的元素, 然后通过`base()`方法转换为对应的正向迭代器it. 然后删除掉it到s.end()的空格.

```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        s.erase(find_if(s.rbegin(), s.rend(), [](char ch){return !std::isspace(ch);}).base(), s.end());
        if(s.empty()) return 0;
        int cnt = 0;
        for(int i = s.size() - 1; i >=0; --i)
        {
            if(s[i] != ' ')
                ++cnt;
            else
                break;
        }
        return cnt;
    }
};
```

