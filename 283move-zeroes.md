Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Note**:

1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

## 双指针

构造指针i，k，指针i向前读取nums的元素，当读取的元素不为0时，添加到k指向的空间中，并且k <- k + 1，当i遍历完之后，在数组末尾(k到nums.size() - 1)补上0就可以了，时间复杂度为O(n)，空间为O(1)

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int k = 0;
        for(int i = 0; i < nums.size(); ++i)
        {
            if(nums[i] != 0)
            {
                nums[k++] = nums[i];
            }
        }
        while(k < nums.size())
            nums[k++] = 0;
    }
};
```

