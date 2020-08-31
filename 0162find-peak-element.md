A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

**Example 2:**

```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```

**Follow up:** Your solution should be in logarithmic complexity.

## 分治

首先对于两个端点begin和end,计算mid

这里要利用一个重要的性质

**如果`nums[mid - 1] > nums[mid]`，那么在左边的数组nums[begin]到nums[mid]中，一定存在一个peak！**

**如果`nums[mid + 1] > nums[mid]`，那么在右边的数组nums[mid]到nums[end]中，一定存在一个peak！**

因为假如`nums[mid - 1] > nums[mid]`的话，那么必然这个子数组的最后一段是递减的（因为如果一直递增的话`nums[mid - 1] > nums[mid]`肯定不成立），那么开始递减的第一个元素的前一个元素就是peak。

例如

+ 5 6 7 8 **9** 2, 其中nums[mid] = 2, nums[mid - 1] = 9, peak = indexOf(9)
+ **9** 8 7 6 5 4, 其中nums[mid] = 4, nums[mid - 1] = 5, peak = indexOf(5)
+ 4 5 **6** 3 2 1, 其中nums[mid] = 1, nums[mid - 1] = 2, peak = indexOf(6)

而当`nums[mid - 1] < nums[mid]`时，可能有peak 也可能没有peak，我们直接不考虑这一部分，因为即使不考虑这一个可能出现的peak，也能保证最后找到至少1个peak

+ 1 2 3 4 5 6, 其中nums[mid] = 6, nums[mid - 1] = 5, 不存在peak
+ 2 **3** 1 4 5 6, 其中nums[mid] = 6, nums[mid - 1] = 5, peak = indexOf(3)

所以，根据上面的讨论

+ 如果`nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]`,那么显然mid是一个peak
+ 如果`nums[mid] < nums[mid - 1]`， peak在左边的数组nums[begin]到nums[mid]中
+ 如果`nums[mid] > nums[mid - 1]`，peak在右边的数组nums[mid]到nums[end]中

这里，为了保证mid-1和mid+1不越界，所以要保证end - begin >=2

当end - begin == 1 或end - begin == 0时，显然peak = nums[begin] > nums[end] ? begin : end;

这样的算法每次花常数时间使数组规模减少一半，`T(n) = T(n/2) +O(1)`，显然是对数复杂度

```c++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        return findPeakElement(nums,0,nums.size() - 1);
    }
private:
    int findPeakElement(const vector<int>& nums, int begin, int end)
    {
        if(begin == end || end - begin == 1)
        {
            return nums[begin] > nums[end] ? begin : end;  //recursion boundary
        }
        int mid = begin + (end - begin) / 2;
        int midLeft = nums[mid - 1];
        int midRight = nums[mid + 1];
        if(midLeft < nums[mid] && midRight < nums[mid])
            return mid;
        if(midLeft > nums[mid])
        {
            return findPeakElement(nums,begin,mid);
        }
        if(midRight > nums[mid])
        {
            return findPeakElement(nums,mid,end);
        }
        throw string("recursion should return before!");
        return -1;   
    }
};
```

