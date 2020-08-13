Given an array *nums* containing *n* + 1 integers where each integer is between 1 and *n* (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

**Example 1:**

```
Input: [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: [3,1,3,4,2]
Output: 3
```

**Note:**

1. You **must not** modify the array (assume the array is read only).
2. You must use only constant, *O*(1) extra space.
3. Your runtime complexity should be less than *O*(*n*2).
4. There is only one duplicate number in the array, but it could be repeated more than once.

## 1 暴力法

我知道肯定会超时, 但是找出又满足不能modify数组的, 又要常数复杂度的, 又要小于n\^2的实在做不到(流下了不学无术的泪水😭)

本来想用分治做的,但是没想出来(一直在想分为两个子数组之后如何在O(n)时间内找出两个子数组的重复元素?后来看答案发现这个思路错了)

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        for(int i = 0; i < nums.size(); ++i)
            for(int j = i + 1; j < nums.size(); ++j)
                if(nums[i] == nums[j])
                    return nums[i];
        return -1;
    }
};
```

## 2 分治

这个题用分治确实可以做， 但是不是将数组划分为两个子数组，而是将duplicate的元素所在的区间一步步划分。

假设low = 1, high = n, mid= (low + high)/2

现在统计各个段的元素个数，

```c++
			int cnt_smaller_than_mid = 0;
            int cnt_larger_than_mid = 0;
            int cnt_equal_mid = 0;
            for(auto i : nums)
            {
                if(i < mid && i >= low)
                    ++cnt_smaller_than_mid;
                if(i > mid && i <= high)
                    ++cnt_larger_than_mid;
                if(i == mid)
                    ++cnt_equal_mid;
            }
```

如果等于mid的元素个数大于1，那么mid肯定是duplicate的

如果小于mid但大于等于low的元素个数 大于 mid - low，那么重复的元素肯定在[low,mid)中

如果大于mid但小于等于high的元素个数大于high - mid，那么重复的元素肯定在(mid,low]中，

再对[low,mid-1]或[mid  + 1,low]采取相同的分治方法，最后求出答案，复杂度O(nlogn)

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int low = 1;
        int high = nums.size() - 1;
        while(low <= high)
        {
            int mid = low + (high - low) / 2;
            int cnt_smaller_than_mid = 0;
            int cnt_larger_than_mid = 0;
            int cnt_equal_mid = 0;
            for(auto i : nums)
            {
                if(i < mid && i >= low)
                    ++cnt_smaller_than_mid;
                if(i > mid && i <= high)
                    ++cnt_larger_than_mid;
                if(i == mid)
                    ++cnt_equal_mid;
            }
            if(cnt_equal_mid > 1)
                return mid;
            else if(cnt_smaller_than_mid > mid - low)
                high = mid - 1;
            else if(cnt_larger_than_mid > high - mid)
                low = mid + 1;
        }
        return -1;
    }
};
```

## 3 快慢指针

将这个数组看成一个链表,每个nums[i]为第i个元素的下一个值.

因为没有指针指向nums[0]\(数组中没有元素是0), 所以nums[0]是表头.

因为数组中存在duplicate, 即两个值同时指向数组中的某个值. 所以, 这个链表中一定存在环. 类似于"6"这样的链表

利用快慢指针即可找到环的起点.