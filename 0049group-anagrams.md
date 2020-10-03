Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

**Example 1:**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example 2:**

```
Input: strs = [""]
Output: [[""]]
```

**Example 3:**

```
Input: strs = ["a"]
Output: [["a"]]
```

 

**Constraints:**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lower-case English letters.

## 1 暴力双重hashmap

外层hashmap储存相同anagram的所有string.(内层的hashmap作为key, `list<string>`作为value)

内层hashmap用来判断两个字符串是不是anagram. (相同的字符出现相同的次数)

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<Map<Character, Integer>, List<String>> mp = new HashMap<>();
        for(int i = 0; i < strs.length; ++i)
        {
            Map<Character, Integer> strmp = str2map(strs[i]);
            mp.putIfAbsent(strmp, new ArrayList<String>());
            mp.get(strmp).add(strs[i]);
        }
        List<List<String>> llist = new ArrayList<>();
        for(List<String> list : mp.values())
        {
            llist.add(list);
        }
        return llist;
    }
    private Map<Character, Integer> str2map(String str)
    {
        Map<Character, Integer> ans = new HashMap<>();
        for(int i = 0; i < str.length(); ++i)
        {
            ans.putIfAbsent(str.charAt(i),0);
            ans.put(str.charAt(i), ans.get(str.charAt(i)) + 1);
        }
        return ans;
    }
}
```

## 2 用char[26]判断是不是anagram

看了题解之后感觉我自己写的双重hashmap的时间复杂度和空间复杂度都和答案一样. 不知道为什么那么慢, 才超过5%. 可能是因为hashmap太多.

把内层的hashmap改成数组cnt.

通过判断cnt来判断两个string是不是anagram.

还需要把char[] cnt转化成String 才可以作为hashmap的键. 这里不能用int[]. 因为相同的int数组转化为string结果不确定.

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> mp = new HashMap<>();
        for(String str : strs)
        {
            char[] cnt = new char[26];
            for(char i : str.toCharArray())
            {
                ++cnt[i - 'a'];
            }
            String strcnt = String.valueOf(cnt);
            mp.putIfAbsent(strcnt, new ArrayList<>());
            mp.get(strcnt).add(str);
        }
        return new ArrayList<>(mp.values());
    }
    
}
```

