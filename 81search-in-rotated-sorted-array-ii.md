Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`).

You are given a target value to search. If found in the array return `true`, otherwise return `false`.

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

**Follow up:**

- This is a follow up problem to [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/), where `nums` may contain duplicates.
- Would this affect the run-time complexity? How and why?

## 1 暴力

我也不想使用暴力, 但是这个重复元素加进来想不出来怎么应对

> [1,3,1,1,1,1] tar=3 , 和[1,1,1,1,3,1] tar=3中,  nums[mid], nums.front, nums.back都为1, 所以无法判断tar在数组的左半部分和右半部分. 只能暴力

```c++
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        for(int i : nums)
            if(target == i) return true;
        return false;
    }
};
```

## 2 改进的改进的二分查找

上面二分失败的原因是nums[mid], nums.front, nums.back都相等, 导致的无法判断tar在数组的左半部分和右半部分. 

下面分析, 如果nums[mid]和nums.front, nums.back不等(即使nums.front == nums.back也可以), 那么就能判断nums[mid]在左上升的子数组中还是nums[mid]在右上升的子数组中.  那接下来也好根据tar的值和nums[mid]判断是在左半数组或是在右半数组中了. 使用和leetcode 33相同的方法即可

所以问题的关键变成了如何确保nums[mid]和nums.front, nums.back不等, 假设对于迭代每一次的起点和终点begin, end, 可以从begin向右边去重, end向左边去重

```c++
while(nums[begin] == nums[begin+1]) ++begin;
while(nums[end] == nums[end-1]) --end;
```

这样去重之后的数组绝对不可能出现nums[mid]和nums[begin]相等或nums[mid]和nums[end]相等的情况了

```c++
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        if(nums.empty())
            return false;
        else
        {
            int i = 0;
            int j = nums.size() - 1;
            int front = nums.front();
            int back = nums.back();
            while(i <= j)
            {
                while(i < nums.size() - 1 && nums[i] == nums[i+1]) ++i;
                while(j > 0 && nums[j] == nums[j-1]) --j;
                int mid = i + (j - i) / 2;
                if(target == nums[mid])
                    return true;
                
                bool midInLeft = (nums[mid] >= front);
                if(midInLeft)
                {
                    if(target >= front && target <= nums[mid])
                    {
                        return regularBinarySearch(nums, target, i, mid - 1) != -1;
                    }
                    else
                    {
                        i = mid + 1;
                        if(i > j) break;  //if not break nums[i] may be illegal address
                        front = nums[i];
                    }
                }
                else
                {
                    if(target >= nums[mid] && target <= back)
                    {
                        return regularBinarySearch(nums, target, mid + 1, j) != -1;
                    }
                    else
                    {
                        j = mid - 1;
                        if(j < i) break;  //if not break nums[j] may be illegal address
                        back = nums[j];
                    }
                }
            }
            return false;
        }
    }
private:
    int regularBinarySearch(const vector<int>& nums, int target, int left, int right)
    {
        int i = left;
        int j = right;
        while(i <= j)
        {
            int mid = i + (j - i) / 2;
            if(nums[mid] == target)
                return mid;
            if(nums[mid] < target)
            {
                i = mid + 1;
            }
            else
            {
                j = mid - 1;
            }
        }
        return -1;
    }
};

```



