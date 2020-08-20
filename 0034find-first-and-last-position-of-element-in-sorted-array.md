Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

If the target is not found in the array, return `[-1, -1]`.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

 

**Constraints:**

- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- `nums` is a non decreasing array.
- `-10^9 <= target <= 10^9`

## 二分查找

题目等价于寻找第一个大于等于target的index left和最后一个小于等于target的index right

如果nums中没有target，则必有 left > right，返回[-1, -1]即可

如果有target，则返回[left, right]即为所求

而寻找left和right用广义的二分查找即可解决

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.size() == 0)
            return vector<int>({-1,-1});
        if(target < nums.front() || target > nums.back())
        {
            return vector<int>({-1,-1});
        }
        int left = getLeftIndex(nums,target);
        int right = getRightIndex(nums,target);
        if(left > right)
        {
            return vector<int>({-1,-1});
        }
        return vector<int>({left,right});
    }
private:
    int getLeftIndex(const vector<int>& nums, int target)
    {
        int i = 0;
        int j = nums.size() - 1;
        while(i <= j)
        {
            int mid = i + (j - i) / 2;
            if(nums[mid] >= target)
            {
                j = mid - 1;
            }
            else
            {
                i = mid + 1;
            }
        }
        return i;
    }
    int getRightIndex(const vector<int>& nums, int target)
    {
        int i = 0;
        int j = nums.size() - 1;
        while(i <= j)
        {
            int mid = i + (j - i) / 2;
            if(nums[mid] <= target)
            {
                i = mid + 1;
            }
            else
            {
                j = mid - 1;
            }
        }
        return j;
    }
};
```



