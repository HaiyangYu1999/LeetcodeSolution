Implement [strStr()](http://www.cplusplus.com/reference/cstring/strstr/).

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**Clarification:**

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's [strstr()](http://www.cplusplus.com/reference/cstring/strstr/) and Java's [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)).

## 1 调用c++ string::find

注意当返回值为`string::npos`时返回-1即可

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int a =  haystack.find(needle);
        if(a == string::npos)
            return -1;
        return a;
    }
};
```

## 2 KMP算法

假设i为被查找字符串的指针，j为pattern的指针

kmp算法的关键在于如何求得数组next，其中next[i] 表示 pattern中从0到i的子数组中最大相同前后缀的长度

例如 pattern= "abcabcd"的next数组为[0,0,0,1,2,3,0]

next[3] = 1 因为 next[0]到next[3]中最大相同前后缀长度为1 ~~a~~bc~~a~~

next[4] = 2 因为 next[0]到next[4]最大相同前后缀长度为2 ~~ab~~c~~ab~~

next[5] = 3 因为 next[0]到next[5]最大相同前后缀长度为3 ~~abc~~ ~~abc~~

next[6] = 0 因为next[0]到next[6] 没有公共的前后缀 

对于任意的pattern，只要构造出next数组，就可以很容易的使用kmp算法（如何递推的构造next见下文）

在字符串str `abcabcabcd` 中查找pattern `abcabcd` ，对于`str[i] == pattern[j]`的情况，同时自加i和j，当查找到`str[i] ！= pattern[j]`的时候，i指针不动，j指针减为next[j-1]继续查找

> abcabc**a**bcd       若第i个元素不同，则指针i不动，指针j回退到next[j-1] 
>
> abcabc**d **             此例中i仍然指向str中第二个a，j回退到pattern的第3个元素

> abcabc**a**bcd 此时直接从pattern第3个元素开始比较，至于前三个元素不用比较也能判断相等，next的性质使得这样的移动是安全的
>
> ​      abc**a**bcd       j此时变为3，

下面还有一个问题，如此一来，pattern相对str向右偏移了3位，那么相对于暴力解法少了下面两种检查的情况，会不会落下这两种可能的解？

> 　a**b**cabcabcd      ab**c**abcabcd
>
> ​      **a**bcabcd   　　　**a**bcabcd

事实上，中间少比较的两种情况也不可能是问题的解（可以证明，如果pattern前j -1位如果和str匹配，并且错后1位也匹配的话，那么前j-1位肯定是全一个相同的字符，如果是错后2位后也完全相同，那么前j-1位肯定是全是一个包含单一的2位子字符串的字符串e.g. "ababababa"，而这样的字符串向右偏移与str配对时偏移量必然小于等于2，因此不可能错后大于2位）



以上证明了kmp算法的正确性，下面描述如何求next数组

递推的求next，假如next[k - 1]已知，求next[k] 。

以下将字符串pattern简写为P

假设next[k-1] = j，这也就意味着 (P[0],...,P[j-1]) == (P[k-j],...,P[k-1])，最理想的情况是P[k] == P[j]，那这样显然next[k] = j+1

#### **下面是递推求next数组的精髓所在，也是kmp算法最大的难点**

如果P[k] ！= P[j]，那么就意味着next[k] <= next[k - 1] ，相同的前后缀只能从(P[0],...,P[j-1])的子序列中找，则需要找到长度更短的长度s，使得**(P[0],...,P[s-1]) == (P[k-s],...,P[k-1])** , 并且希望P[k] == P[s]。

根据我们假设的条件 (P[0],...,P[j-1]) == (P[k-j],...,P[k-1])，那么取一下子序列，必然有**(P[j - s],...,P[j-1]) == (P[k-s],...,P[k-1])**

所以根据两个加粗的等式，我们得到**(P[0],...,P[s-1]) == (P[j - s],...,P[j-1])**

**这个式子说明了对于数组(P[0],...,P[j - 1])，有长度为s的公共前后缀！**

所以 **s=next[j - 1]**，对应代码中的`len = next[len - 1];`,但是这并没有保证公共前后缀的最后一个元素P[s]和P[k]相等，所以要递归的带入s = next[s - 1],直到最后一个元素P[s]和P[k]相等。同时注意s==0时跳出循环，可以直接判断P[s]和P[k]是否相等得到next[k]为0还是1.



```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        return KMPfind(haystack,needle);
    }
private:
    
vector<int> getNext(const string& P)
{
    if(P.empty()) return vector<int>();
    vector<int> next(P.size(), 0);
    next.front() = 0;
    int len = 0;
    for(int i = 1; i < P.size(); ++i)
    {
        while(len > 0 && P[i] != P[len])
        {
            len = next[len - 1];
        }
        if(P[i] == P[len])
        {
            ++len;
        }
        next[i] = len;
    }
    return next;
}
int KMPfind(const string& str, const string& pattern)
{
    if(pattern.size() > str.size())
        return -1;
    if(pattern.empty())
        return 0;
    int i = 0; //str index
    int j = 0; //pattern index
    auto next = getNext(pattern);
    while(i < str.size())
    {
        if(str[i] == pattern[j])
        {
            ++i;
            ++j;
        }
        else
        {
            if(j == 0)
            {
                ++i;
            }
            else
            {
                j = next[j - 1];
            }
        }
        if(j == pattern.size()) return i - j;
    }
    return -1;
}
};
```



