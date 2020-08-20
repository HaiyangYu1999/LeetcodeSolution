Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

## 双指针1

指针i维护不是val的数组元素, 指针j遍历数组判断值是否为val,如果不是就添加到数组前部i维护的子数组中

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i = 0;
        int size = nums.size();
        for(int j = 0; j < size; ++j)
        {
            if(nums[j] != val)
            {
                nums[i++] = nums[j];
            }
        }
        return i;
        
    }
};
```

这样, 需要做size- s次交换(s为数组中等于val的元素个数)。如果数组中等于val的值较少，效率低

## 双指针2

这次，指针j从数组结尾开始遍历，指针i从数组头部记录。

i开始遍历，当nums[i] == val 时，用尾部的值替换nums[i]，同时j自减, nums[i] = nums[--j]。再次判断新的nums[i]是否等于val

当i>=j时停止。（注意nums[j]不是最后一个元素，nums[j-1]是）

这样只需要进行交换s次(s为数组中等于val的元素个数) 如果数组中等于val的值较少，效率高

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i = 0;
        int j = nums.size();
        while(i<j)
        {
            if(nums[i] == val)
            {
                nums[i] = nums[--j];
            }
            else
            {
                ++i;
            }
        }
        return j;
    }
};
```

