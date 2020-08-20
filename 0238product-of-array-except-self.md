Given an array `nums` of *n* integers where *n* > 1,  return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

**Example:**

```
Input:  [1,2,3,4]
Output: [24,12,8,6]
```

**Constraint:** It's guaranteed that the product of the elements of any prefix or suffix of the array (including the whole array) fits in a 32 bit integer.

**Note:** Please solve it **without division** and in O(*n*).

**Follow up:**
Could you solve it with constant space complexity? (The output array **does not** count as extra space for the purpose of space complexity analysis.)

## 原地修改

主要想法就是花费O(n)的时间构造2个数组`prev` 和 `next`. `prev[i]` 存储nums从0到i的积, `next[i]`存储nums从i到size-1的积. 所以结果`ans[i] = prev[i-1] * next[i+1]`. **但是这个方法还是我断断续续想了一天才想出来的. 之前都在纠结于 *在不用除法的情况下* 怎样根据算出来的第i个值ans[i]和数组nums来推算第i+1个值 **.可见一开始就在错误的道路上越走越远了😠😠

但是这个需要花费2n的额外空间. 一个很自然的想法就是, **利用原数组nums储存一个数组prev, 利用返回的数组ans储存另一个数组next**.

首先在ans中储存next, 在nums中储存prev

然后从0到size-1地计算 `ans[i] = nums[i-1] * ans[i+1]`. 当i = 0 或 i = size - 1 时退化为`ans[0] = ans[1] ` 或 `ans[size-1] =nums[size-2] ` 

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> ans(nums.size());
        int tmp = 1;
        for(int i = nums.size() - 1; i > -1; --i)
        {
            tmp *= nums[i];
            ans[i] = tmp;
        }
        tmp = 1;
        for(int i = 0; i < nums.size(); ++i)
        {
            tmp *= nums[i];
            nums[i] = tmp;
        }
        for(int i = 0; i < nums.size(); ++i)
        {
            if(i == 0)
                ans[i] = ans[i+1];
            else if(i == nums.size() - 1)
                ans[i] = nums[i-1];
            else
                ans[i] = nums[i-1] * ans[i+1];
        }
        return ans;
    }
};
```

