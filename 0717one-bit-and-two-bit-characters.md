We have two special characters. The first character can be represented by one bit `0`. The second character can be represented by two bits (`10` or `11`).

Now given a string represented by several bits. Return whether the last character must be a one-bit character or not. The given string will always end with a zero.

**Example 1:**

```
Input: 
bits = [1, 0, 0]
Output: True
Explanation: 
The only way to decode it is two-bit character and one-bit character. So the last character is one-bit character.
```



**Example 2:**

```
Input: 
bits = [1, 1, 1, 0]
Output: False
Explanation: 
The only way to decode it is two-bit character and two-bit character. So the last character is NOT one-bit character.
```



**Note:**

`1 <= len(bits) <= 1000`.

`bits[i]` is always `0` or `1`.

## 暴力

i从左到右依次的解码遍历, 若为1则 i = i + 2, is1bit赋值为false, 0则为i = i + 1, is1bit赋值为true. 遍历完后返回is1bit即可

```c++
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        if(bits.empty() || bits.back() == 1)
        {
            throw string("Invalid BitStream");
        }
        int i = 0;
        bool is1bit;
        while(i < bits.size())
        {
            if(bits[i] == 0)
            {
                is1bit = true;
                i += 1;
            }
            else
            {
                is1bit = false;
                i += 2;
            }
        }
        return is1bit;
    }
};
```

