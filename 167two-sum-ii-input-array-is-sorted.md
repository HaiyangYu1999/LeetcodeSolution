Given an array of integers that is already ***sorted in ascending order\***, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**

- Your returned answers (both index1 and index2) are not zero-based.
- You may assume that each input would have *exactly* one solution and you may not use the *same* element twice.

**Example:**

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

## 1 暴力法

两个指针i,j从两边开始判断两数和是否等于target，相等就返回两个指针

若大于tar，右边指针递减，小于tar，左边指针递增

假如这个numbers不是排好序的，也能用hashtable在O(n)时间算出，既然这个是已经排好序的，应该有低于O(n)的方法，但我想了半天没想出来低于O(n)的方法，假如有高人想到，欢迎指点.

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for(int i = 0, j = numbers.size() - 1; i < j;)
        {
            if(numbers[i] + numbers[j] == target)
                return vector<int>{i + 1,j + 1};
            if(numbers[i] + numbers[j] > target)
            {
                --j;
            }
            else
                ++i;
        }
        return vector<int>{-1,-1};
    }
};
```

