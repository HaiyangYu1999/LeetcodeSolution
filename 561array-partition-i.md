Given an array of **2n** integers, your task is to group these integers into **n** pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

**Example 1:**

```
Input: [1,4,3,2]

Output: 4
Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).
```



**Note:**

1. **n** is a positive integer, which is in the range of [1, 10000].
2. All the integers in the array will be in the range of [-10000, 10000].

## 数学推导

要获得各个min(ai, bi)的和最大, 最小值和第2小的值肯定是组成一个pair, 第3小的和第4小的组成一个pair, ..., 以此类推. (否则, 如果最小的和第二小的元素不是一个pair, 那么把他们组成一个pair会获得更大的sum值)

排序后计算nums[0]+nums[2]+...+nums[2*n - 2]的值即可.

```c++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int res = 0;
        for(int i = 0; i < n/2; ++i)
        {
            res += nums[2*i];
        }
        return res;
    }
};
```

