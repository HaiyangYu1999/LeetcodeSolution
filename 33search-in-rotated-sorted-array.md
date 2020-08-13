Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

## 改进的二分查找

注意到,虽然被rotated了,但是仍然有这样的结构, nums = [a1,a2,a3,....,an,b1,b2,...bm],其中b1<b2<...<bm<a1<a2<...<an。可以利用这样的结构完成二分查找

储存两个变量front和back记录数组第一个和最后一个值

两个变量 i，j记录数组左右端点

先计算mid和mid位置(位于子数组ai还是子数组bi)

+ 假如mid>front，即mid位于数组ai中，nums=[a1,a2,a3,....,mid...,an,b1,b2,...bm]，

> + 假如target比nums[mid]大或比back小, 那么target肯定在mid右边，右边的数组仍然是一个rotatedArray，这时改变左右端点为右边的子数组的端点(mid+1, j)，改变front和back，递归的使用相同的步骤进行二分查找（这里要注意更新完左右端点i,j后要检查是否i<=j，否则更新front=nums[i]容易下标溢出）
>
> + 而当target大于front小于nums[mid]时，那么target肯定在mid左边, 而mid又在子数组ai中，所以mid左边的数组是没有rotated的。此时用普通的二分查找得到结果即可

+ 假如mid<front，即mid位于数组bn中，nums=[a1,a2,a3,....,an,b1,b2,...,mid,...,bm]

> + 假如target比nums[mid]小或比back大，那么target肯定在mid左边，左边的数组仍然是一个rotatedArray，这是改变左右端点为左边子数组端点(i,mid - 1), 改变front和back，使用相同的步骤查找（也要注意改变back时下标溢出）
> + 而当target比num[mid]大或比back小，那么target肯定在mid右边，右边的数组是没有rotated的，二分查找即可。

此题关键在于二分之后的左半数组和右半数组一定有一个是有序的.

总时间复杂度仍然为O(log n)

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty())
            return -1;
        bool isRotated = (nums.front() > nums.back());
        if(!isRotated)
        {
            return regularBinarySearch(nums, target, 0, nums.size() - 1);
        }
        else
        {
            int i = 0;
            int j = nums.size() - 1;
            int front = nums.front();
            int back = nums.back();
            while(i <= j)
            {
                int mid = i + (j - i) / 2;
                if(target == nums[mid])
                    return mid;
                
                bool midInLeft = (nums[mid] >= front);
                if(midInLeft)
                {
                    if(target >= front && target <= nums[mid])
                    {
                        return regularBinarySearch(nums, target, i, mid - 1);
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
                        return regularBinarySearch(nums, target, mid + 1, j);
                    }
                    else
                    {
                        j = mid - 1;
                        if(j < i) break;  //if not break nums[j] may be illegal address
                        back = nums[j];
                    }
                }
            }
            return -1;
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

