Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

注意，要判断nums的长度，如果是0则会报错

## 1.暴力解法

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        auto nextInteger = nums.begin();
        auto bg = nextInteger;
        auto ed = nextInteger;
        while(bg != nums.end())
        {
            while((ed != nums.end()) && (*bg == *ed)) //get the range of same integer
            {
                ++ed;
            }
            ++bg;
            nextInteger = nums.erase(bg,ed);
            bg = nextInteger;                       //find the next integer range
            ed = nextInteger;
        }
        return nums.size();
    }
};
```

构造两个指针bg ed, 通过不断比较找到相同的数字范围，删除两个指针之间的重复元素. 然后去比较下一个相同的数字的范围并删除。以此类推。 由于每一次调用erase的开销太大，复杂度高O(n^2)

## 2. 双指针

思路是两个指针i，j  指针i遍历数组判断是否读取的是一个新的整数，指针j维护数组前j个互不相同的整数，判断出新的整数之后就将这个新的整数添加到j+1的位置。同时更新j。复杂度O(n)

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) 
    {
        int len = nums.size();
        if(len < 2)
        {
            return len;
        }
        int j = 0;
        for(int i = 1; i < len; ++i)
        {
            if(nums[j] != nums[i])
            {
                nums[++j] = nums[i];
            }
        }
        return j+1;
    }
};
```

