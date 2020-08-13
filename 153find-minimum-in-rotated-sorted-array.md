Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

You may assume no duplicate exists in the array.

**Example 1:**

```
Input: [3,4,5,1,2] 
Output: 1
```

**Example 2:**

```
Input: [4,5,6,7,0,1,2]
Output: 0
```

## 分治算法

如果数组的第一个元素小于最后一个元素，那么这个数组是未被rotated的，第一个值肯定是最小值

如果将这个数组从中间分开,左边和右边两个子数组肯定有一个是未被rotated的，能直接计算出最小值，另外一个rotated的数组可以再递归的分为两个数组来计算最小值，最后，取（左数组最小值，中间值，右数组最小值）的最小值作为数组最小值。

复杂度有`T(n) = T(n/2) + O(1)`, 即O(log n)的复杂度

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        return findMin(nums, 0, nums.size() - 1);
    }
private:
    int findMin(const vector<int>& nums, int begin, int end)
    {
        if(begin > end) return INT_MAX; //recursion boundary
        //check if sorted with no rotation
        if(nums[begin] < nums[end])
        {
            return nums[begin];
        }
        int mid = begin + (end - begin) / 2;
        int leftMin = findMin(nums, begin, mid - 1);
        int midMin = nums[mid];
        int rightMin = findMin(nums,mid + 1, end);
        return min(midMin, min(leftMin, rightMin));
    }
};
```

