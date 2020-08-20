Given a **non-empty** array of digits representing a non-negative integer, increment one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contains a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

**Example 1:**

```
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```

**Example 2:**

```
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
```



## 分类讨论

+　正常情况下末尾加1即可
+　若后几位是9，把9替换为0并且在前一位加1
+　若全是9，返回一个digits.size()+1的100...000的vector

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        bool isAll9 = true;
        for(int i = 0; i < digits.size(); ++i)
        {
            if(digits[i] != 9)
            {
                isAll9 = false;
                break;
            }
        }
        if(isAll9)
        {
            vector<int> vc;
            vc.push_back(1);
            vc.insert(vc.end(), digits.size(), 0);
            return vc;
        }
        else
        {
            for(int i = digits.size() - 1; i >= 0; --i)
            {
                if(digits[i] != 9)
                {
                    ++digits[i];
                    break;
                }
                else
                {
                    digits[i] = 0;
                }
            }
            return digits;
        }
    }
};
```

