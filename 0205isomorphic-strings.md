Given two strings ***s\*** and ***t\***, determine if they are isomorphic.

Two strings are isomorphic if the characters in ***s\*** can be replaced to get ***t\***.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

**Example 1:**

```
Input: s = "egg", t = "add"
Output: true
```

**Example 2:**

```
Input: s = "foo", t = "bar"
Output: false
```

**Example 3:**

```
Input: s = "paper", t = "title"
Output: true
```

**Note:**
You may assume both ***s\*** and ***t\*** have the same length.

## 1 hashmap

考虑所有的字符从s到t的映射.

如果对于某个i, s[i]映射到t[i], 并且这个映射在map中没有, 就把这个映射`s[i]->t[i]`加入到map中

如果键s[i]在map中存在, 并且该键对应的值**不等于**t[i], 就说明做映射s肯定变换不到t上去. 所以返回false. 例如(`aa -> ab`, 没法构造一个映射既有`a->a`又有`a->b`)

如果键s[i]在map中存在, 并且该键对应的值**等于**t[i], 跳过, 判断i+1的映射.

同时, 要双向判断映射是否存在. 例如`aa 和 ab` 如果只判断`ab`是否能映射到`aa`是成立的, 但是`aa`却没办法映射到`ab`. 所以要双向判断, 只有在都能互相映射的时候才返回true

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        return isIsomorphic0(s, t) && isIsomorphic0(t, s);
    }
    private boolean isIsomorphic0(String s, String t)
    {
        if(s == null || t == null)
            return false;
        if(s.length() != t.length())
            return false;
        final int n = s.length();
        Map<Character, Character> map = new HashMap<>();
        for(int i = 0; i < n; ++i)
        {
            char ch1 = s.charAt(i);
            char ch2 = t.charAt(i);
            if(map.containsKey(ch1))
            {
                if(map.get(ch1) != ch2)
                    return false;
            }
            else
            {
                map.put(ch1, ch2);
            }
        }
        return true;
    }
    
}
```

## 2 使用字符本身作为hash

即对于ch1 = s[i]和ch2 = t[i], 获取ch1在s中第一次出现的位置, 和ch2在t中第一次出现的位置, 如果两者不相等的话, 就返回false. 

**这个方法和上面的方法相比, 不占用额外空间, 但是时间复杂度会高. 上面的是O(n), 这一个最坏情况下是O(n^2)**

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if(s == null || t == null)
            return false;
        if(s.length() != t.length())
            return false;
        final int n = s.length();
        for(int i = 0; i < n; ++i)
        {
            if(s.indexOf(s.charAt(i)) != t.indexOf(t.charAt(i)))
                return false;
        }
        return true;
    }
}
```

