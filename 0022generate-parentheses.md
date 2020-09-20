Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

## 1 回溯 + 剪枝

考虑到一共有n个左括号, n个右括号. string长度为2n

所以每个位置都可以有放置"("和")"两种选择. **然后要注意传递两个参数(左括号剩余数量, 右括号剩余数量). 如果左括号用完了就只能放置右括号了. 两个括号都用完了就把字符串current加入到结果ans中**

**对于剪枝, 如果发现放置了k个括号的字符串中, 右括号多于左括号, 那么就剪枝掉它. 因为无论第k个括号之后的括号怎么放, 都不是合法的括号字符串!** 例如, n = 3, 前三个括号为"())", 无论剩下的的括号(2个"(", 1个'')'')怎么排序都不对了.

```c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        generateParenthesis("", ans, n, n);
        return ans;
    }
private:
    void generateParenthesis(string current, vector<string>& ans, int leftParenthesis, int rightParenthesis)
    {
        if(leftParenthesis + rightParenthesis == 0)
        {
            ans.push_back(current);
            return;
        }
        if(!precheck(current))
            return;
        if(leftParenthesis > 0)
            generateParenthesis(current + "(", ans, leftParenthesis - 1, rightParenthesis);
        if(rightParenthesis > 0)
            generateParenthesis(current + ")", ans, leftParenthesis, rightParenthesis - 1);
    }
    bool precheck(string current)
    {
        return (count(current.begin(), current.end(), '(') < count(current.begin(), current.end(), ')')) ? false : true;
    }
};
```

## 2 回溯 + 剪枝 (改进)

观察上面的剪枝过程, 每一次都要计算current字符串中的左括号数量和右括号数量. **事实上这两个根本不用计算, 直接从参数leftParenthesis和rightParenthesis中获得即可!**

```c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        generateParenthesis("", ans, n, n);
        return ans;
    }
private:
    void generateParenthesis(string current, vector<string>& ans, int leftParenthesis, int rightParenthesis)
    {
        if(leftParenthesis + rightParenthesis == 0)
        {
            ans.push_back(current);
            return;
        }
        if(leftParenthesis > rightParenthesis)
            return;
        if(leftParenthesis > 0)
            generateParenthesis(current + "(", ans, leftParenthesis - 1, rightParenthesis);
        if(rightParenthesis > 0)
            generateParenthesis(current + ")", ans, leftParenthesis, rightParenthesis - 1);
    }
};
```

## 3 直接生成

这个答案摘自leetcode官网(https://leetcode.com/problems/generate-parentheses/solution/). 

它没有用回溯法一个元素一个元素的构造, 而是观察解的结构. 

发现任意一个解都能用 `'(' + left + ')' + right`来表示. 其中, left是含有`c`组括号组成的所有合法字符串, right是含有`n - c - 1`组括号组成的所有合法字符串. c 属于 [0, n - 1]. 

下面举例说明, n = 5,

> `()()((()))`可以表示为 left = "", right = `()((()))` , c = 0
>
> `((()()()))`可以表示为 left = `(()()())` , right = "", c = 4
>
> `(()(()))()`可以表示为 left = `()(())`, right = `()` ,   c = 3

所以只需要遍历c, 即可得到所有的解

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList();
        if (n == 0) {
            ans.add("");
        } else {
            for (int c = 0; c < n; ++c)
                for (String left: generateParenthesis(c))
                    for (String right: generateParenthesis(n-1-c))
                        ans.add("(" + left + ")" + right);
        }
        return ans;
    }
}
```

