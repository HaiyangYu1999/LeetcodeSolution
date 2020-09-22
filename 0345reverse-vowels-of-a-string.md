Write a function that takes a string as input and reverse only the vowels of a string.

**Example 1:**

```
Input: "hello"
Output: "holle"
```

**Example 2:**

```
Input: "leetcode"
Output: "leotcede"
```

**Note:**
The vowels does not include the letter "y".

## 双指针

两个指针left, right

首先使得left指向第1个vowel, right指向倒数第1个vowel, 然后交换他们.

再使得left指向第2个vowel, right指向倒数第2个vowel, 交换他们

直到 left >= right

注意的细节是, 每一次找到left和right之后都要判断是否满足条件! 不能只在while循环写`while(left < right)`

```c++
class Solution {
public:
    string reverseVowels(string s) {
        int left = 0;
        int right = s.size() - 1;
        while(right > left)
        {
            while(left < s.size() && !isVowels(s[left]))
                ++left;
            while(right > -1 && !isVowels(s[right]))
                --right;
            if(left > right)
                break;
            swap(s[left], s[right]);
            ++left;
            --right;
        }
        return s;
    }
private:
    bool isVowels(char ch)
    {
        char a = std::tolower(ch);
        return a == 'a' || a == 'e' || a == 'i' || a == 'o' || a == 'u';
    }
};
```

