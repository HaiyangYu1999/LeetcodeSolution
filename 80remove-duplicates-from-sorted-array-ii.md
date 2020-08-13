Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that duplicates appeared at most *twice* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,1,2,3,3],

Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

## 双指针

和leetcode 26的解法类似, 构造扫描指针j和插入指针i, 当j扫描到满足条件的值时就插入到i中. 

本题需要两个变量存储信息. currentNumber储存当前的值, 来判断是否和前面重复. currentCount储存currentNumber重复的次数. \

首先判断当前的nums[j]和currentNumber是否相等. 若相等则判断重复次数, 若重复的次数超过2次, 则j直接自增. 不超过2次, 则currentCount自增, 并将nums[j++]插入到nums[i++]. 

若不相等, 则更新currentNumber为nums[j], 并将nums[j++]插入到nums[i++]

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() <= 2)
            return nums.size();
        int i = 1;
        int j = 1;
        int currentNumber = nums[0];
        int currentCount = 1;
        while(j < nums.size())
        {
            if(nums[j] != currentNumber)
            {
                currentNumber = nums[j];
                currentCount = 1;
                nums[i++] = nums[j++];
            }
            else
            {
                ++currentCount;
                if(currentCount == 2)
                {
                    nums[i++] = nums[j++];
                }
                else if(currentCount > 2)
                {
                    ++j;
                }
            }
        }
        return i;
    }
};
```

