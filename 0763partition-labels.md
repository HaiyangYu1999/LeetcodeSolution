A string `S` of lowercase English letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

 

**Example 1:**

```
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

 

**Note:**

- `S` will have length in range `[1, 500]`.
- `S` will consist of lowercase English letters (`'a'` to `'z'`) only.

## 双指针

首先构造一个hashmap, 存储字符i和字符i最后一次出现的位置.(换成int数组也可以, 更省空间).

解题思路不愿意写了, 贴一个吧https://www.cnblogs.com/yuzhenzero/p/10713254.html

假设我们有一个片段是符合要求的，我们给这个片段设一个标签叫`a`，那字母`a`最后出现的位置肯定也在这个片段中（如果不在这个片段中，而在其他的地方出现了，就不符合题目一个字母只在一个片段出现的要求）。

在两个`a`之间，一般来讲也会有其他字母，同理，在这期间其他字母最后一次出现也要包含在这个片段中，这就会导致这个符合要求的片段扩张一部分。举个例子，原字符串是`“abccaddbeffe”`，则第一个符合要求的片段是`“abccaddb”`。

利用上述这个思想，我们可以使用如下方法来解题：

1. 构造一个数组，存放给定字符串`s`中，每个字符最后出现的索引
2. 设置两个指针`start`和`end`分别表示符合要求的片段的开始索引和结束索引
3. 按字符遍历字符串，不断更新`end`的值，直到`i == end`说明已经搜寻到一个符合要求的片段了，此时重置`start`的值

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> list = new ArrayList<>();
        Map<Character, Integer> map = new HashMap<>();
        for(int i = S.length() - 1; i > -1; --i)
        {
            map.putIfAbsent(S.charAt(i), i);
        }
        int begin = 0;
        int end = 0;
        while(begin < S.length())
        {
            for(int i = begin; i <= end; ++i)
            {
                end = Math.max(map.get(S.charAt(i)), end);
            }
            int len = end - begin + 1;
            list.add(len);
            begin = end = end + 1;
        }
        return list;
    }
}
```

